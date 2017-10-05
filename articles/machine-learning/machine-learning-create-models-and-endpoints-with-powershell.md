---
title: "Skapa flera modeller från ett experiment | Microsoft Docs"
description: "Använda PowerShell för att skapa flera Machine Learning-modeller och web service-slutpunkter med samma algoritm men olika utbildning datauppsättningar."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: 21d8c1ee0877df8d317d5a14131dc574fa5303c4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a><span data-ttu-id="699a6-103">Skapa många Machine Learning-modeller och webbtjänstslutpunkter från ett experiment med PowerShell</span><span class="sxs-lookup"><span data-stu-id="699a6-103">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>
<span data-ttu-id="699a6-104">Här är ett vanligt problem i machine learning: du vill skapa många modeller som har samma utbildning arbetsflöde och använda samma algoritm, men olika utbildning datauppsättningar som indata.</span><span class="sxs-lookup"><span data-stu-id="699a6-104">Here's a common machine learning problem: You want to create many models that have the same training workflow and use the same algorithm, but have different training datasets as input.</span></span> <span data-ttu-id="699a6-105">Den här artikeln visar hur du gör detta på skalan i Azure Machine Learning Studio med bara ett enda experiment.</span><span class="sxs-lookup"><span data-stu-id="699a6-105">This article shows you how to do this at scale in Azure Machine Learning Studio using just a single experiment.</span></span>

<span data-ttu-id="699a6-106">Anta exempelvis att du äger ett globala cykel uthyrning franchise företag.</span><span class="sxs-lookup"><span data-stu-id="699a6-106">For example, let's say you own a global bike rental franchise business.</span></span> <span data-ttu-id="699a6-107">Du vill skapa en regressionsmodell för att förutsäga uthyrning-begäran som bygger på historiska data.</span><span class="sxs-lookup"><span data-stu-id="699a6-107">You want to build a regression model to predict the rental demand based on historic data.</span></span> <span data-ttu-id="699a6-108">Du har 1 000 uthyrning platser över hela världen och du har lagrat en datauppsättning för varje plats som innehåller viktiga funktioner, till exempel datum, tid, väder och trafik som är specifika för varje plats.</span><span class="sxs-lookup"><span data-stu-id="699a6-108">You have 1,000 rental locations across the world and you've collected a dataset for each location that includes important features such as date, time, weather, and traffic that are specific to each location.</span></span>

<span data-ttu-id="699a6-109">Du kan lära din modell som en gång med hjälp av en sammanfogad version av alla datauppsättningar på alla platser.</span><span class="sxs-lookup"><span data-stu-id="699a6-109">You could train your model once using a merged version of all the datasets across all locations.</span></span> <span data-ttu-id="699a6-110">Men eftersom var och en av dina platser har en unik miljö, en bättre metod är att träna din regressionsmodell separat med datauppsättningen för varje plats.</span><span class="sxs-lookup"><span data-stu-id="699a6-110">But because each of your locations has a unique environment, a better approach would be to train your regression model separately using the dataset for each location.</span></span> <span data-ttu-id="699a6-111">På så sätt kan varje tränad modell kan ta hänsyn till olika store storlekar, volym, geografi, ifyllning, cykel eget trafik miljö *etc.*.</span><span class="sxs-lookup"><span data-stu-id="699a6-111">That way, each trained model could take into account the different store sizes, volume, geography, population, bike-friendly traffic environment, *etc.*.</span></span>

<span data-ttu-id="699a6-112">Som kan vara det bästa sättet, men du vill inte skapa 1 000 utbildning experiment i Azure Machine Learning med var och en representerar en unik plats.</span><span class="sxs-lookup"><span data-stu-id="699a6-112">That may be the best approach, but you don't want to create 1,000 training experiments in Azure Machine Learning with each one representing a unique location.</span></span> <span data-ttu-id="699a6-113">Förutom att det är en överväldigande aktivitet, är det också verkar vara ganska ineffektiv eftersom varje experiment skulle ha samma komponenter förutom utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="699a6-113">Besides being an overwhelming task, it's also seems pretty inefficient since each experiment would have all the same components except for the training dataset.</span></span>

<span data-ttu-id="699a6-114">Lyckligtvis vi kan göra detta med hjälp av den [Azure Machine Learning API omtränings](machine-learning-retrain-models-programmatically.md) och automatisera aktiviteten med [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span><span class="sxs-lookup"><span data-stu-id="699a6-114">Fortunately, we can accomplish this by using the [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) and automating the task with [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span></span>

> [!NOTE]
> <span data-ttu-id="699a6-115">Vi ska minska antalet platser från 1 000 till 10 för att göra exemplet körs snabbare.</span><span class="sxs-lookup"><span data-stu-id="699a6-115">To make our sample run faster, we'll reduce the number of locations from 1,000 to 10.</span></span> <span data-ttu-id="699a6-116">Men samma principer och procedurer som gäller för 1 000 platser.</span><span class="sxs-lookup"><span data-stu-id="699a6-116">But the same principles and procedures apply to 1,000 locations.</span></span> <span data-ttu-id="699a6-117">Den enda skillnaden är att du förmodligen vill tror följande PowerShell-skript som körs parallellt om du vill att träna från 1 000 datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="699a6-117">The only difference is that if you want to train from 1,000 datasets you probably want to think of running the following PowerShell scripts in parallel.</span></span> <span data-ttu-id="699a6-118">Hur du gör är utanför omfattningen för den här artikeln, men du kan hitta exempel på PowerShell flertrådsteknik på Internet.</span><span class="sxs-lookup"><span data-stu-id="699a6-118">How to do that is beyond the scope of this article, but you can find examples of PowerShell multi-threading on the Internet.</span></span>  
> 
> 

## <a name="set-up-the-training-experiment"></a><span data-ttu-id="699a6-119">Ställ in utbildning experimentet</span><span class="sxs-lookup"><span data-stu-id="699a6-119">Set up the training experiment</span></span>
<span data-ttu-id="699a6-120">Vi använder ett exempel [träningsexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) som vi redan har skapat i den [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="699a6-120">We're going to use an example [training experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) that we've already created in the [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="699a6-121">Öppna experimentet i din [Azure Machine Learning Studio](https://studio.azureml.net) arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="699a6-121">Open this experiment in your [Azure Machine Learning Studio](https://studio.azureml.net) workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="699a6-122">För att kunna följa det här exemplet kan du vill använda en standard arbetsyta i stället för en kostnadsfri arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="699a6-122">In order to follow along with this example, you may want to use a standard workspace rather than a free workspace.</span></span> <span data-ttu-id="699a6-123">Vi ska skapa en slutpunkt för varje kund - 10-slutpunkter – totalt och som kräver en standard arbetsyta eftersom en kostnadsfri arbetsyta är begränsat till 3 slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="699a6-123">We'll be creating one endpoint for each customer - for a total of 10 endpoints - and that will require a standard workspace since a free workspace is limited to 3 endpoints.</span></span> <span data-ttu-id="699a6-124">Om du bara har en kostnadsfri arbetsyta bara ändra skripten nedan så att bara 3 platser.</span><span class="sxs-lookup"><span data-stu-id="699a6-124">If you only have a free workspace, just modify the scripts below to allow for only 3 locations.</span></span>
> 
> 

<span data-ttu-id="699a6-125">Experimentet använder en **importera Data** modulen att importera utbildning datauppsättningen *customer001.csv* från ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="699a6-125">The experiment uses an **Import Data** module to import the training dataset *customer001.csv* from an Azure storage account.</span></span> <span data-ttu-id="699a6-126">Antar vi att vi har samlat in utbildning datauppsättningar från alla cykel uthyrning platser och lagrat dem på samma plats för blob storage med filnamn som sträcker sig från *rentalloc001.csv* till *rentalloc10.csv*.</span><span class="sxs-lookup"><span data-stu-id="699a6-126">Let's assume we have collected training datasets from all bike rental locations and stored them in the same blob storage location with file names ranging from *rentalloc001.csv* to *rentalloc10.csv*.</span></span>

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

<span data-ttu-id="699a6-128">Observera att en **Web Service utdata** modulen har lagts till i **Träningsmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="699a6-128">Note that a **Web Service Output** module has been added to the **Train Model** module.</span></span>
<span data-ttu-id="699a6-129">När experimentet distribueras som en webbtjänst slutpunkten som är kopplade till att utdata ska returnera den tränade modellen i formatet av en .ilearner-fil.</span><span class="sxs-lookup"><span data-stu-id="699a6-129">When this experiment is deployed as a web service, the endpoint associated with that output will return the trained model in the format of a .ilearner file.</span></span>

<span data-ttu-id="699a6-130">Även Observera att vi ställa in en web service-parameter för URL-Adressen som den **importera Data** modulen används.</span><span class="sxs-lookup"><span data-stu-id="699a6-130">Also note that we set up a web service parameter for the URL that the **Import Data** module uses.</span></span> <span data-ttu-id="699a6-131">Detta gör att vi kan använda parametern för att ange enskilda utbildning datauppsättningar för att träna modellen för varje plats.</span><span class="sxs-lookup"><span data-stu-id="699a6-131">This allows us to use the parameter to specify individual training datasets to train the model for each location.</span></span>
<span data-ttu-id="699a6-132">Det finns andra sätt som vi gick har gjort det, till exempel använder en SQL-fråga med en parameter för web service för att hämta data från en SQL Azure-databas, eller bara använda en **webbtjänst** modulen att skicka in en datamängd till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="699a6-132">There are other ways we could have done this, such as using a SQL query with a web service parameter to get data from a SQL Azure database, or simply using a  **Web Service Input** module to pass in a dataset to the web service.</span></span>

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

<span data-ttu-id="699a6-134">Nu ska vi kör utbildning experimentet använder standardvärdet *rental001.csv* som utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="699a6-134">Now, let's run this training experiment using the default value *rental001.csv* as the training dataset.</span></span> <span data-ttu-id="699a6-135">Om du visar utdata från den **utvärdera** modulen (klicka på utdata och välj **visualisera**), du kan se vi hämta ett hyfsad prestanda för *AUC* = 0.91.</span><span class="sxs-lookup"><span data-stu-id="699a6-135">If you view the output of the **Evaluate** module (click the output and select **Visualize**), you can see we get a decent performance of *AUC* = 0.91.</span></span> <span data-ttu-id="699a6-136">Vi är nu redo att distribuera en webbtjänst utanför utbildning experimentet.</span><span class="sxs-lookup"><span data-stu-id="699a6-136">At this point, we're ready to deploy a web service out of this training experiment.</span></span>

## <a name="deploy-the-training-and-scoring-web-services"></a><span data-ttu-id="699a6-137">Distribuera utbildning och bedömningen webbtjänster</span><span class="sxs-lookup"><span data-stu-id="699a6-137">Deploy the training and scoring web services</span></span>
<span data-ttu-id="699a6-138">Om du vill distribuera webbtjänsten utbildning klickar du på den **konfigurera Web Service** under arbetsytan för experimentet och välj **distribuera webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="699a6-138">To deploy the training web service, click the **Set Up Web Service** button below the experiment canvas and select **Deploy Web Service**.</span></span> <span data-ttu-id="699a6-139">Anropa den här webbtjänsten ”” cykel uthyrning utbildning ”.</span><span class="sxs-lookup"><span data-stu-id="699a6-139">Call this web service ""Bike Rental Training".</span></span>

<span data-ttu-id="699a6-140">Nu ska vi distribuera bedömningsprofil webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="699a6-140">Now we need to deploy the scoring web service.</span></span>
<span data-ttu-id="699a6-141">Detta gör vi klickar på **konfigurera Web Service** under arbetsytan och välj **förutsägande webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="699a6-141">To do this, we can click **Set Up Web Service** below the canvas and select **Predictive Web Service**.</span></span> <span data-ttu-id="699a6-142">Detta skapar ett bedömningsprofil experiment.</span><span class="sxs-lookup"><span data-stu-id="699a6-142">This creates a scoring experiment.</span></span>
<span data-ttu-id="699a6-143">Vi behöver göra några mindre justeringar så att den fungerar som en webbtjänst som att ta bort etikett kolumnen ”cnt” från indata och utdata till endast instans-id och motsvarande begränsa förväntad värde.</span><span class="sxs-lookup"><span data-stu-id="699a6-143">We'll need to make a few minor adjustments to make it work as a web service, such as removing the label column "cnt" from the input data and limiting the output to only the instance id and the corresponding predicted value.</span></span>

<span data-ttu-id="699a6-144">Du kan spara arbetet, kan du bara öppna den [prediktivt experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) i galleriet som redan förberetts.</span><span class="sxs-lookup"><span data-stu-id="699a6-144">To save yourself that work, you can simply open the [predictive experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in the Gallery that's already been prepared.</span></span>

<span data-ttu-id="699a6-145">Om du vill distribuera webbtjänsten kör prediktivt experiment och klicka sedan på den **distribuera webbtjänsten** knapp under arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="699a6-145">To deploy the web service, run the predictive experiment, then click the **Deploy Web Service** button below the canvas.</span></span> <span data-ttu-id="699a6-146">Bedömningsprofil webbtjänsten ”cykel uthyrning bedömningen” namn ”.</span><span class="sxs-lookup"><span data-stu-id="699a6-146">Name the scoring web service "Bike Rental Scoring"".</span></span>

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a><span data-ttu-id="699a6-147">Skapa 10 slutpunkter för identiska webbtjänster med PowerShell</span><span class="sxs-lookup"><span data-stu-id="699a6-147">Create 10 identical web service endpoints with PowerShell</span></span>
<span data-ttu-id="699a6-148">Den här webbtjänsten levereras med en standardslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="699a6-148">This web service comes with a default endpoint.</span></span> <span data-ttu-id="699a6-149">Men vi har inte intresserad standardslutpunkten eftersom den inte uppdateras.</span><span class="sxs-lookup"><span data-stu-id="699a6-149">But we're not as interested in the default endpoint since it can't be updated.</span></span> <span data-ttu-id="699a6-150">Vi behöver göra är att skapa 10 ytterligare slutpunkter, ett för varje plats.</span><span class="sxs-lookup"><span data-stu-id="699a6-150">What we need to do is to create 10 additional endpoints, one for each location.</span></span> <span data-ttu-id="699a6-151">Vi ska göra detta med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="699a6-151">We'll do this with PowerShell.</span></span>

<span data-ttu-id="699a6-152">Först måste konfigurerar vi våra PowerShell-miljö:</span><span class="sxs-lookup"><span data-stu-id="699a6-152">First, we set up our PowerShell environment:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

<span data-ttu-id="699a6-153">Kör följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="699a6-153">Then, run the following PowerShell command:</span></span>

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

<span data-ttu-id="699a6-154">Nu har vi skapat 10 slutpunkter och alla innehålla samma tränad modell tränas på *customer001.csv*.</span><span class="sxs-lookup"><span data-stu-id="699a6-154">Now we've created 10 endpoints and they all contain the same trained model trained on *customer001.csv*.</span></span> <span data-ttu-id="699a6-155">Du kan visa dem i Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="699a6-155">You can view them in the Azure Management Portal.</span></span>

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a><span data-ttu-id="699a6-157">Uppdatera slutpunkterna för att använda separata tränings datauppsättningar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="699a6-157">Update the endpoints to use separate training datasets using PowerShell</span></span>
<span data-ttu-id="699a6-158">Nästa steg är att uppdatera slutpunkterna med modeller tränas unikt på varje enskild kunddata.</span><span class="sxs-lookup"><span data-stu-id="699a6-158">The next step is to update the endpoints with models uniquely trained on each customer's individual data.</span></span> <span data-ttu-id="699a6-159">Men vi måste först skapa dessa modeller från den **cykel uthyrning utbildning** webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="699a6-159">But first we need to produce these models from the **Bike Rental Training** web service.</span></span> <span data-ttu-id="699a6-160">Vi går tillbaka till den **cykel uthyrning utbildning** webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="699a6-160">Let's go back to the **Bike Rental Training** web service.</span></span> <span data-ttu-id="699a6-161">Vi behöver anropa sin BES slutpunkt 10 gånger med 10 olika utbildning datauppsättningar för att producera 10 olika modeller.</span><span class="sxs-lookup"><span data-stu-id="699a6-161">We need to call its BES endpoint 10 times with 10 different training datasets in order to produce 10 different models.</span></span> <span data-ttu-id="699a6-162">Vi använder den **InovkeAmlWebServiceBESEndpoint** PowerShell-cmdlet för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="699a6-162">We'll use the **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet to do this.</span></span>

<span data-ttu-id="699a6-163">Du måste också ange autentiseringsuppgifter för blob storage-konto i `$configContent`, nämligen på fälten `AccountName`, `AccountKey` och `RelativeLocation`.</span><span class="sxs-lookup"><span data-stu-id="699a6-163">You will also need to provide credentials for your blob storage account into `$configContent`, namely, at the fields `AccountName`, `AccountKey` and `RelativeLocation`.</span></span> <span data-ttu-id="699a6-164">Den `AccountName` kan vara något av ditt kontonamn som visas i den **klassiska Azure-hanteringsportalen** (*lagring* fliken).</span><span class="sxs-lookup"><span data-stu-id="699a6-164">The `AccountName` can be one of your account names, as seen in the **Classic Azure Management Portal** (*Storage* tab).</span></span> <span data-ttu-id="699a6-165">När du klickar på ett lagringskonto dess `AccountKey` hittar du genom att trycka på **hantera åtkomstnycklar** längst ned och kopiera den *primära åtkomstnyckeln*.</span><span class="sxs-lookup"><span data-stu-id="699a6-165">Once you click on a storage account, its `AccountKey` can be found by pressing the **Manage Access Keys** button at the bottom and copying the *Primary Access Key*.</span></span> <span data-ttu-id="699a6-166">Den `RelativeLocation` är sökväg i förhållande till din lagring där en ny modell ska lagras.</span><span class="sxs-lookup"><span data-stu-id="699a6-166">The `RelativeLocation` is the path relative to your storage where a new model will be stored.</span></span> <span data-ttu-id="699a6-167">Till exempel sökvägen `hai/retrain/bike_rental/` i skriptet nedan pekar på en behållare med namnet `hai`, och `/retrain/bike_rental/` är undermappar.</span><span class="sxs-lookup"><span data-stu-id="699a6-167">For instance, the path `hai/retrain/bike_rental/` in the script below points to a container named `hai`, and `/retrain/bike_rental/` are subfolders.</span></span> <span data-ttu-id="699a6-168">För närvarande kan du skapa undermappar via portalen UI, men det finns [flera Azure-lagringsutforskare](../storage/common/storage-explorers.md) som gör det möjligt att göra detta.</span><span class="sxs-lookup"><span data-stu-id="699a6-168">Currently, you cannot create subfolders through the portal UI, but there are [several Azure Storage Explorers](../storage/common/storage-explorers.md) that allow you to do so.</span></span> <span data-ttu-id="699a6-169">Vi rekommenderar att du skapar en ny behållare i lagring att lagra nya tränade modeller (.ilearner-filer) på följande sätt: från lagringssidan klickar du på den **Lägg till** längst ned och ger den namnet `retrain`.</span><span class="sxs-lookup"><span data-stu-id="699a6-169">It is recommended that you create a new container in your storage to store the new trained models (.ilearner files) as follows: from your storage page, click on the **Add** button at the bottom and name it `retrain`.</span></span> <span data-ttu-id="699a6-170">Sammanfattningsvis nödvändiga ändringar i skriptet nedan berör `AccountName`, `AccountKey` och `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span><span class="sxs-lookup"><span data-stu-id="699a6-170">In summary, the necassary changes to the script below pertain to `AccountName`, `AccountKey` and `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span></span>

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> <span data-ttu-id="699a6-171">BES-slutpunkten är det enda läget som stöds för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="699a6-171">The BES endpoint is the only supported mode for this operation.</span></span> <span data-ttu-id="699a6-172">RR kan inte användas för att framställa tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="699a6-172">RRS cannot be used for producing trained models.</span></span>
> 
> 

<span data-ttu-id="699a6-173">Som du ser ovan, i stället för att konstruera 10 olika BES jobbet json konfigurationsfiler, vi skapa config-sträng i stället och dynamiskt feed det till den *jobConfigString* parameter för den  **InvokeAmlWebServceBESEndpoint** cmdlet, eftersom det inte finns egentligen inget behov av att behålla en kopia på disken.</span><span class="sxs-lookup"><span data-stu-id="699a6-173">As you can see above, instead of constructing 10 different BES job configuration json files, we dynamically create the config string instead and feed it to the *jobConfigString* parameter of the **InvokeAmlWebServceBESEndpoint** cmdlet, since there is really no need to keep a copy on disk.</span></span>

<span data-ttu-id="699a6-174">Om allt går bra efter en stund bör du se 10 .ilearner filer från *model001.ilearner* till *model010.ilearner*, i ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="699a6-174">If everything goes well, after a while you should see 10 .ilearner files, from *model001.ilearner* to *model010.ilearner*, in your Azure storage account.</span></span> <span data-ttu-id="699a6-175">Nu vi är redo att uppdatera våra 10 bedömningen slutpunkter för webbtjänster med dessa modeller med hjälp av den **korrigering AmlWebServiceEndpoint** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="699a6-175">Now we're ready to update our 10 scoring web service endpoints with these models using the **Patch-AmlWebServiceEndpoint** PowerShell cmdlet.</span></span> <span data-ttu-id="699a6-176">Kom ihåg igen att vi kan bara korrigering av icke-standard-slutpunkter som vi programmässigt skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="699a6-176">Remember again that we can only patch the non-default endpoints we programmatically created earlier.</span></span>

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

<span data-ttu-id="699a6-177">Detta bör köra ganska snabbt.</span><span class="sxs-lookup"><span data-stu-id="699a6-177">This should run fairly quickly.</span></span> <span data-ttu-id="699a6-178">När körningen har slutförts ska vi har skapat 10 förutsägande slutpunkter för webbtjänster, som innehåller en tränad modell tränas unikt på specifika dataset till en plats för uthyrning, allt från en enda träningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="699a6-178">When the execution finishes, we'll have successfully created 10 predictive web service endpoints, each containing a trained model uniquely trained on the dataset specific to a rental location, all from a single training experiment.</span></span> <span data-ttu-id="699a6-179">Om du vill kontrollera detta, kan du anropa dessa slutpunkter som använder den **InvokeAmlWebServiceRRSEndpoint** cmdlet, vilket ger dem med samma indata och du bör se olika förutsägelse resultat eftersom modeller tränas med olika utbildning mängder.</span><span class="sxs-lookup"><span data-stu-id="699a6-179">To verify this, you can try calling these endpoints using the **InvokeAmlWebServiceRRSEndpoint** cmdlet, providing them with the same input data, and you should expect to see different prediction results since the models are trained with different training sets.</span></span>

## <a name="full-powershell-script"></a><span data-ttu-id="699a6-180">Fullständig PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="699a6-180">Full PowerShell script</span></span>
<span data-ttu-id="699a6-181">Här är lista med fullständig källkoden:</span><span class="sxs-lookup"><span data-stu-id="699a6-181">Here's the listing of the full source code:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
