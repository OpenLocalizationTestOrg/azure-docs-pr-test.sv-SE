---
title: "aaaUsing Import/Export av Data i Azure Machine Learning-webbtjänster | Microsoft Docs"
description: "Lär dig hur toouse hello importera Data och exportera Data moduler toosend och ta emot data från en webbtjänst."
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
ms.openlocfilehash: 176380259b15cb338ede61c7f28ba2296b35dd52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploying-azure-ml-web-services-that-use-data-import-and-data-export-modules"></a><span data-ttu-id="7ca30-103">Distribuera Azure ML-webbtjänster som använder moduler för dataimport och dataexport</span><span class="sxs-lookup"><span data-stu-id="7ca30-103">Deploying Azure ML web services that use Data Import and Data Export modules</span></span>

<span data-ttu-id="7ca30-104">När du skapar en prediktivt experiment kan du vanligtvis lägga till en webbtjänst och utgående.</span><span class="sxs-lookup"><span data-stu-id="7ca30-104">When you create a predictive experiment, you typically add a web service input and output.</span></span> <span data-ttu-id="7ca30-105">När du distribuerar hello experiment kan konsumenterna skicka och ta emot data från webbtjänsten hello via hello indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="7ca30-105">When you deploy hello experiment, consumers can send and receive data from hello web service through hello inputs and outputs.</span></span> <span data-ttu-id="7ca30-106">För vissa program, kan en konsument vara tillgänglig från en datafeed eller redan finns i en extern datakälla, till exempel Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="7ca30-106">For some applications, a consumer's data may be available from a data feed or already reside in an external data source such as Azure Blob storage.</span></span> <span data-ttu-id="7ca30-107">I dessa fall kan behöver de inte läsa och skriva data med hjälp av web service in- och utdataenheter.</span><span class="sxs-lookup"><span data-stu-id="7ca30-107">In these cases, they do not need read and write data using web service inputs and outputs.</span></span> <span data-ttu-id="7ca30-108">De kan i stället använda hello Batch Execution Service BES-tooread data från hello-datakälla med hjälp av en modul importera Data och skriva hello bedömningen resultat tooa olika plats med hjälp av en modul exportera Data.</span><span class="sxs-lookup"><span data-stu-id="7ca30-108">They can, instead, use hello Batch Execution Service (BES) tooread data from hello data source using an Import Data module and write hello scoring results tooa different data location using an Export Data module.</span></span>

<span data-ttu-id="7ca30-109">hello importera Data och exportera data moduler kan läsa från och skriva toovarious uppgifter platser, till exempel en URL via HTTP, en Hive-fråga, en Azure SQL-databas, Azure Table storage, Azure Blob storage Data Feed tillhandahåller eller en lokal SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="7ca30-109">hello Import Data and Export data modules, can read from and write toovarious data locations such as a Web URL via HTTP, a Hive Query, an Azure SQL database, Azure Table storage, Azure Blob storage, a Data Feed provide, or an on-premises SQL database.</span></span>

<span data-ttu-id="7ca30-110">Det här avsnittet använder hello ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” exempel och förutsätter datamängden hello har redan lästs i en Azure SQL-tabell med namnet censusdata.</span><span class="sxs-lookup"><span data-stu-id="7ca30-110">This topic uses hello "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample and assumes hello dataset has already been loaded into an Azure SQL table named censusdata.</span></span>

## <a name="create-hello-training-experiment"></a><span data-ttu-id="7ca30-111">Skapa hello utbildning experiment</span><span class="sxs-lookup"><span data-stu-id="7ca30-111">Create hello training experiment</span></span>
<span data-ttu-id="7ca30-112">När du öppnar hello ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” exempel den använder hello exempeldatamängd vuxna inventering intäkter binär klassificering.</span><span class="sxs-lookup"><span data-stu-id="7ca30-112">When you open hello "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" sample it uses hello sample Adult Census Income Binary Classification dataset.</span></span> <span data-ttu-id="7ca30-113">Och hello experiment hello arbetsytan ser liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="7ca30-113">And hello experiment in hello canvas will look similar toohello following image:</span></span>

![Inledande konfiguration av hello experiment.](./media/machine-learning-web-services-that-use-import-export-modules/initial-look-of-experiment.png)

<span data-ttu-id="7ca30-115">tooread hello data från hello Azure SQL-tabellen:</span><span class="sxs-lookup"><span data-stu-id="7ca30-115">tooread hello data from hello Azure SQL table:</span></span>

1. <span data-ttu-id="7ca30-116">Ta bort hello dataset modul.</span><span class="sxs-lookup"><span data-stu-id="7ca30-116">Delete hello dataset module.</span></span>
2. <span data-ttu-id="7ca30-117">Skriv import i sökrutan för hello komponenter.</span><span class="sxs-lookup"><span data-stu-id="7ca30-117">In hello components search box, type import.</span></span>
3. <span data-ttu-id="7ca30-118">Hello resultatlistan lägga till en *importera Data* modulen toohello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7ca30-118">From hello results list, add an *Import Data* module toohello experiment canvas.</span></span>
4. <span data-ttu-id="7ca30-119">Ansluta utdata från hello *importera Data* hello modulindata av hello *Rensa Data som saknas* modul.</span><span class="sxs-lookup"><span data-stu-id="7ca30-119">Connect output of hello *Import Data* module hello input of hello *Clean Missing Data* module.</span></span>
5. <span data-ttu-id="7ca30-120">Välj i egenskapsrutan, **Azure SQL Database** i hello **datakällan** listrutan.</span><span class="sxs-lookup"><span data-stu-id="7ca30-120">In properties pane, select **Azure SQL Database** in hello **Data Source** dropdown.</span></span>
6. <span data-ttu-id="7ca30-121">I hello **Databasservernamnet**, **databasnamnet**, **användarnamn**, och **lösenord** ange hello lämplig information för din databas.</span><span class="sxs-lookup"><span data-stu-id="7ca30-121">In hello **Database server name**, **Database name**, **User name**, and **Password** fields, enter hello appropriate information for your database.</span></span>
7. <span data-ttu-id="7ca30-122">Ange hello följande fråga i hello databasen fråga fältet.</span><span class="sxs-lookup"><span data-stu-id="7ca30-122">In hello Database query field, enter hello following query.</span></span>
   
     <span data-ttu-id="7ca30-123">Välj [ålder]</span><span class="sxs-lookup"><span data-stu-id="7ca30-123">select [age],</span></span>
   
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
     <span data-ttu-id="7ca30-124">från dbo.censusdata;</span><span class="sxs-lookup"><span data-stu-id="7ca30-124">from dbo.censusdata;</span></span>
8. <span data-ttu-id="7ca30-125">Hello längst ned på hello experimentet klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-125">At hello bottom of hello experiment canvas, click **Run**.</span></span>

## <a name="create-hello-predictive-experiment"></a><span data-ttu-id="7ca30-126">Skapa hello prediktivt experiment</span><span class="sxs-lookup"><span data-stu-id="7ca30-126">Create hello predictive experiment</span></span>
<span data-ttu-id="7ca30-127">Nästa ställa in hello prediktivt experiment som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ca30-127">Next you set up hello predictive experiment from which you deploy your web service.</span></span>

1. <span data-ttu-id="7ca30-128">Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänst (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-128">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Predictive Web Service [Recommended]**.</span></span>
2. <span data-ttu-id="7ca30-129">Ta bort hello *webbtjänst* och *Web Service utdata moduler* från hello prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="7ca30-129">Remove hello *Web Service Input* and *Web Service Output modules* from hello predictive experiment.</span></span> 
3. <span data-ttu-id="7ca30-130">Skriv export i sökrutan för hello komponenter.</span><span class="sxs-lookup"><span data-stu-id="7ca30-130">In hello components search box, type export.</span></span>
4. <span data-ttu-id="7ca30-131">Hello resultatlistan lägga till en *exportera Data* modulen toohello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="7ca30-131">From hello results list, add an *Export Data* module toohello experiment canvas.</span></span>
5. <span data-ttu-id="7ca30-132">Ansluta utdata från hello *Poängmodell* hello modulindata av hello *exportera Data* modul.</span><span class="sxs-lookup"><span data-stu-id="7ca30-132">Connect output of hello *Score Model* module hello input of hello *Export Data* module.</span></span> 
6. <span data-ttu-id="7ca30-133">Välj i egenskapsrutan, **Azure SQL Database** i hello data mål listrutan.</span><span class="sxs-lookup"><span data-stu-id="7ca30-133">In properties pane, select **Azure SQL Database** in hello data destination dropdown.</span></span>
7. <span data-ttu-id="7ca30-134">I hello **Databasservernamnet**, **databasnamnet**, **Server användarkontonamnet**, och **serverlösenord** ange hello lämplig information för din databas.</span><span class="sxs-lookup"><span data-stu-id="7ca30-134">In hello **Database server name**, **Database name**, **Server user account name**, and **Server user account password** fields, enter hello appropriate information for your database.</span></span>
8. <span data-ttu-id="7ca30-135">I hello **kommaavgränsad lista över kolumner toobe sparade** anger poängsatta etiketter.</span><span class="sxs-lookup"><span data-stu-id="7ca30-135">In hello **Comma separated list of columns toobe saved** field, type Scored Labels.</span></span>
9. <span data-ttu-id="7ca30-136">I hello **Data tabell namnfältet**, Skriv dbo. ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="7ca30-136">In hello **Data table name field**, type dbo.ScoredLabels.</span></span> <span data-ttu-id="7ca30-137">Om hello tabellen inte finns, skapas den när hello experiment körs eller hello webbtjänst anropas.</span><span class="sxs-lookup"><span data-stu-id="7ca30-137">If hello table does not exist, it is created when hello experiment is run or hello web service is called.</span></span>
10. <span data-ttu-id="7ca30-138">I hello **kommaavgränsad lista med kolumner som datatable** anger ScoredLabels.</span><span class="sxs-lookup"><span data-stu-id="7ca30-138">In hello **Comma separated list of datatable columns** field, type ScoredLabels.</span></span>

<span data-ttu-id="7ca30-139">När du skriver ett program att anrop hello slutliga webbtjänst kanske du vill toospecify en annan fråga eller mål indatatabellen vid körning.</span><span class="sxs-lookup"><span data-stu-id="7ca30-139">When you write an application that calls hello final web service, you may want toospecify a different input query or destination table at run time.</span></span> <span data-ttu-id="7ca30-140">tooconfigure dessa indata och utdata, använda hello Webbtjänstparametrar funktionen tooset hello *importera Data* modulen *datakällan* egenskap och hello *exportera Data* läge data mål-egenskap.</span><span class="sxs-lookup"><span data-stu-id="7ca30-140">tooconfigure these inputs and outputs, use hello Web Service Parameters feature tooset hello *Import Data* module *Data source* property and hello *Export Data* mode data destination property.</span></span>  <span data-ttu-id="7ca30-141">Mer information om Webbtjänstparametrar finns hello [AzureML Webbtjänstparametrar post](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) på hello Cortana Intelligence och Machine Learning-bloggen.</span><span class="sxs-lookup"><span data-stu-id="7ca30-141">For more information on Web Service Parameters, see hello [AzureML Web Service Parameters entry](https://blogs.technet.microsoft.com/machinelearning/2014/11/25/azureml-web-service-parameters/) on hello Cortana Intelligence and Machine Learning Blog.</span></span>

<span data-ttu-id="7ca30-142">tooconfigure hello Webbtjänstparametrar för hello importera fråga och hello Måltabell:</span><span class="sxs-lookup"><span data-stu-id="7ca30-142">tooconfigure hello Web Service Parameters for hello import query and hello destination table:</span></span>

1. <span data-ttu-id="7ca30-143">I hello egenskaper för hello *importera Data* modulen, klicka på hello längst hello uppifrån höger om hello **databasfrågan** fältet och välj **anges som parameter för web service**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-143">In hello properties pane for hello *Import Data* module, click hello icon at hello top right of hello **Database query** field and select **Set as web service parameter**.</span></span>
2. <span data-ttu-id="7ca30-144">I hello egenskaper för hello *exportera Data* modulen, klicka på hello längst hello uppifrån höger om hello **Data tabellnamn** fältet och välj **anges som parameter för web service**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-144">In hello properties pane for hello *Export Data* module, click hello icon at hello top right of hello **Data table name** field and select **Set as web service parameter**.</span></span>
3. <span data-ttu-id="7ca30-145">Längst ned hello hello *exportera Data* modulen egenskapsrutan i hello **Webbtjänstparametrar** , på databasfrågan och byta namn på den frågan.</span><span class="sxs-lookup"><span data-stu-id="7ca30-145">At hello bottom of hello *Export Data* module properties pane, in hello **Web Service Parameters** section, click Database query and rename it Query.</span></span>
4. <span data-ttu-id="7ca30-146">Klicka på **Data tabellnamn** och Byt namn på den **tabellen**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-146">Click **Data table name** and rename it **Table**.</span></span>

<span data-ttu-id="7ca30-147">När du är klar experimentet bör se ut ungefär toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="7ca30-147">When you are done, your experiment should look similar toohello following image:</span></span>

![Sista utseendet på experimentet.](./media/machine-learning-web-services-that-use-import-export-modules/experiment-with-import-data-added.png)

<span data-ttu-id="7ca30-149">Du kan nu distribuera hello experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7ca30-149">Now you can deploy hello experiment as a web service.</span></span>

## <a name="deploy-hello-web-service"></a><span data-ttu-id="7ca30-150">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7ca30-150">Deploy hello web service</span></span>
<span data-ttu-id="7ca30-151">Du kan distribuera tooeither en klassisk eller ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="7ca30-151">You can deploy tooeither a Classic or New web service.</span></span>

### <a name="deploy-a-classic-web-service"></a><span data-ttu-id="7ca30-152">Distribuera en klassiska webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7ca30-152">Deploy a Classic Web Service</span></span>
<span data-ttu-id="7ca30-153">toodeploy som en klassiska webbtjänst och skapa ett program tooconsume den:</span><span class="sxs-lookup"><span data-stu-id="7ca30-153">toodeploy as a Classic Web Service and create an application tooconsume it:</span></span>

1. <span data-ttu-id="7ca30-154">Klicka på Kör hello nedre kanten av hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="7ca30-154">At hello bottom of hello experiment canvas, click Run.</span></span>
2. <span data-ttu-id="7ca30-155">När du kör hello har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-155">When hello run has completed, click **Deploy Web Service** and select **Deploy Web Service [Classic]**.</span></span>
3. <span data-ttu-id="7ca30-156">Leta upp din API-nyckel på hello web service instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="7ca30-156">On hello web service dashboard, locate your API key.</span></span> <span data-ttu-id="7ca30-157">Kopiera och spara den toouse senare.</span><span class="sxs-lookup"><span data-stu-id="7ca30-157">Copy and save it toouse later.</span></span>
4. <span data-ttu-id="7ca30-158">I hello **standard Endpoint** tabell genom att klicka på hello **Batch Execution** länk tooopen hello API-hjälpsidan.</span><span class="sxs-lookup"><span data-stu-id="7ca30-158">In hello **Default Endpoint** table, click hello **Batch Execution** link tooopen hello API Help Page.</span></span>
5. <span data-ttu-id="7ca30-159">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-159">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
6. <span data-ttu-id="7ca30-160">Hitta hello på hello API-hjälpsidan **exempelkod** avsnitt på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="7ca30-160">On hello API Help Page, find hello **Sample Code** section at hello bottom of hello page.</span></span>
7. <span data-ttu-id="7ca30-161">Kopiera och klistra in hello C#-exempelkod i Program.cs-filen och ta bort alla referenser toohello blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="7ca30-161">Copy and paste hello C# sample code into your Program.cs file, and remove all references toohello blob storage.</span></span>
8. <span data-ttu-id="7ca30-162">Uppdatera hello värdet för hello *apiKey* variabeln med hello API-nyckel som sparats tidigare.</span><span class="sxs-lookup"><span data-stu-id="7ca30-162">Update hello value of hello *apiKey* variable with hello API key saved earlier.</span></span>
9. <span data-ttu-id="7ca30-163">Leta upp hello begäran deklaration och uppdatera hello värdena för Webbtjänstparametrar som skickas toohello *importera Data* och *exportera Data* moduler.</span><span class="sxs-lookup"><span data-stu-id="7ca30-163">Locate hello request declaration and update hello values of Web Service Parameters that are passed toohello *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="7ca30-164">I så fall måste du använda hello ursprungliga frågan, men definiera ett nytt tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="7ca30-164">In this case, you use hello original query, but define a new table name.</span></span>
   
        var request = new BatchExecutionRequest() 
        {           
            GlobalParameters = new Dictionary<string, string>() {
                { "Query", @"select [age], [workclass], [fnlwgt], [education], [education-num], [marital-status], [occupation], [relationship], [race], [sex], [capital-gain], [capital-loss], [hours-per-week], [native-country], [income] from dbo.censusdata" },
                { "Table", "dbo.ScoredTable2" },
            }
        };
10. <span data-ttu-id="7ca30-165">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="7ca30-165">Run hello application.</span></span> 

<span data-ttu-id="7ca30-166">För slutförande av hello kör läggs en ny tabell toohello databas som innehåller hello bedömningen resultat.</span><span class="sxs-lookup"><span data-stu-id="7ca30-166">On completion of hello run, a new table is added toohello database containing hello scoring results.</span></span>

### <a name="deploy-a-new-web-service"></a><span data-ttu-id="7ca30-167">Distribuera en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7ca30-167">Deploy a New Web Service</span></span>

> [!NOTE] 
> <span data-ttu-id="7ca30-168">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ca30-168">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="7ca30-169">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="7ca30-169">For more information, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="7ca30-170">toodeploy som en ny webbtjänst och skapa ett program tooconsume den:</span><span class="sxs-lookup"><span data-stu-id="7ca30-170">toodeploy as a New Web Service and create an application tooconsume it:</span></span>

1. <span data-ttu-id="7ca30-171">Hello längst ned på hello experimentet klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-171">At hello bottom of hello experiment canvas, click **Run**.</span></span>
2. <span data-ttu-id="7ca30-172">När du kör hello har slutförts klickar du på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-172">When hello run has completed, click **Deploy Web Service** and select **Deploy Web Service [New]**.</span></span>
3. <span data-ttu-id="7ca30-173">Ange ett namn för din webbtjänsten hello distribuera Experiment på sidan och välja en prisavtal, och klicka sedan **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-173">On hello Deploy Experiment page, enter a name for your web service, and select a pricing plan, then click **Deploy**.</span></span>
4. <span data-ttu-id="7ca30-174">På hello **Quickstart** klickar du på **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-174">On hello **Quickstart** page, click **Consume**.</span></span>
5. <span data-ttu-id="7ca30-175">I hello **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-175">In hello **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="7ca30-176">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="7ca30-176">In Visual Studio, create a C# console application: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
7. <span data-ttu-id="7ca30-177">Kopiera och klistra in hello C# exempelkoden i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="7ca30-177">Copy and paste hello C# sample code into your Program.cs file.</span></span>
8. <span data-ttu-id="7ca30-178">Uppdatera hello värdet för hello *apiKey* variabeln med hello **primärnyckel** finns i hello **grundläggande förbrukning info** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7ca30-178">Update hello value of hello *apiKey* variable with hello **Primary Key** located in hello **Basic consumption info** section.</span></span>
9. <span data-ttu-id="7ca30-179">Leta upp hello *scoreRequest* deklaration och uppdatera hello värdena för Webbtjänstparametrar som skickas toohello *importera Data* och *exportera Data* moduler.</span><span class="sxs-lookup"><span data-stu-id="7ca30-179">Locate hello *scoreRequest* declaration and update hello values of Web Service Parameters that are passed toohello *Import Data* and *Export Data* modules.</span></span> <span data-ttu-id="7ca30-180">I så fall måste du använda hello ursprungliga frågan, men definiera ett nytt tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="7ca30-180">In this case, you use hello original query, but define a new table name.</span></span>
   
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
10. <span data-ttu-id="7ca30-181">Kör hello program.</span><span class="sxs-lookup"><span data-stu-id="7ca30-181">Run hello application.</span></span> 

