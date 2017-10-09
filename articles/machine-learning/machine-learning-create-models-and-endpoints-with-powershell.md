---
title: "aaaCreate flera modeller från ett experiment | Microsoft Docs"
description: "Använd PowerShell toocreate flera Machine Learning-modeller och web service-slutpunkter med hello samma algoritm men olika utbildning datauppsättningar."
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
ms.openlocfilehash: 4a258a8ab26395d4169a058520151c860e16e169
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a><span data-ttu-id="a8a42-103">Skapa många Machine Learning-modeller och webbtjänstslutpunkter från ett experiment med PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8a42-103">Create many Machine Learning models and web service endpoints from one experiment using PowerShell</span></span>
<span data-ttu-id="a8a42-104">Här är ett vanligt problem i machine learning: du vill toocreate många modeller som har hello samma utbildning arbetsflödet och Använd hello samma algoritm, men har olika utbildning datauppsättningar som indata.</span><span class="sxs-lookup"><span data-stu-id="a8a42-104">Here's a common machine learning problem: You want toocreate many models that have hello same training workflow and use hello same algorithm, but have different training datasets as input.</span></span> <span data-ttu-id="a8a42-105">Den här artikeln beskrivs hur du toodo detta skalanpassat i Azure Machine Learning Studio med bara ett enda experiment.</span><span class="sxs-lookup"><span data-stu-id="a8a42-105">This article shows you how toodo this at scale in Azure Machine Learning Studio using just a single experiment.</span></span>

<span data-ttu-id="a8a42-106">Anta exempelvis att du äger ett globala cykel uthyrning franchise företag.</span><span class="sxs-lookup"><span data-stu-id="a8a42-106">For example, let's say you own a global bike rental franchise business.</span></span> <span data-ttu-id="a8a42-107">Vill du toobuild regression modellen toopredict hello uthyrning behov baserat på historiska data.</span><span class="sxs-lookup"><span data-stu-id="a8a42-107">You want toobuild a regression model toopredict hello rental demand based on historic data.</span></span> <span data-ttu-id="a8a42-108">Du har 1 000 uthyrning platser över hello world och du har lagrat en datauppsättning för varje plats som innehåller viktiga funktioner, till exempel datum, tid, väder och trafik som är specifika tooeach plats.</span><span class="sxs-lookup"><span data-stu-id="a8a42-108">You have 1,000 rental locations across hello world and you've collected a dataset for each location that includes important features such as date, time, weather, and traffic that are specific tooeach location.</span></span>

<span data-ttu-id="a8a42-109">Du kan lära din modell som en gång med hjälp av en sammanfogad version av alla hello datauppsättningar på alla platser.</span><span class="sxs-lookup"><span data-stu-id="a8a42-109">You could train your model once using a merged version of all hello datasets across all locations.</span></span> <span data-ttu-id="a8a42-110">Men eftersom var och en av dina platser har en unik miljö bättre skulle vara tootrain din regressionsmodell med hello dataset separat för varje plats.</span><span class="sxs-lookup"><span data-stu-id="a8a42-110">But because each of your locations has a unique environment, a better approach would be tootrain your regression model separately using hello dataset for each location.</span></span> <span data-ttu-id="a8a42-111">På så sätt kan varje tränad modell kan beakta kontostorlekar hello olika store, volym, geografi, ifyllning, cykel eget trafik miljö *etc.*.</span><span class="sxs-lookup"><span data-stu-id="a8a42-111">That way, each trained model could take into account hello different store sizes, volume, geography, population, bike-friendly traffic environment, *etc.*.</span></span>

<span data-ttu-id="a8a42-112">Som kan vara hello bästa sättet, men du vill inte toocreate 1 000 utbildning experiment i Azure Machine Learning med var och en representerar en unik plats.</span><span class="sxs-lookup"><span data-stu-id="a8a42-112">That may be hello best approach, but you don't want toocreate 1,000 training experiments in Azure Machine Learning with each one representing a unique location.</span></span> <span data-ttu-id="a8a42-113">Förutom att det är en överväldigande aktivitet, är det också verkar vara ganska ineffektiv eftersom varje experiment skulle ha alla hello samma komponenter förutom hello utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="a8a42-113">Besides being an overwhelming task, it's also seems pretty inefficient since each experiment would have all hello same components except for hello training dataset.</span></span>

<span data-ttu-id="a8a42-114">Lyckligtvis vi kan göra detta med hjälp av hello [Azure Machine Learning API omtränings](machine-learning-retrain-models-programmatically.md) och automatiskt hello aktivitet med [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span><span class="sxs-lookup"><span data-stu-id="a8a42-114">Fortunately, we can accomplish this by using hello [Azure Machine Learning retraining API](machine-learning-retrain-models-programmatically.md) and automating hello task with [Azure Machine Learning PowerShell](machine-learning-powershell-module.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a8a42-115">toomake exemplet körs snabbare, kommer vi minska hello antalet platser från 1 000 too10.</span><span class="sxs-lookup"><span data-stu-id="a8a42-115">toomake our sample run faster, we'll reduce hello number of locations from 1,000 too10.</span></span> <span data-ttu-id="a8a42-116">Men hello samma principer och procedurer gäller too1 000 platser.</span><span class="sxs-lookup"><span data-stu-id="a8a42-116">But hello same principles and procedures apply too1,000 locations.</span></span> <span data-ttu-id="a8a42-117">hello enda skillnaden är att om du vill tootrain från 1 000 datauppsättningar vill du förmodligen toothink körs hello följande PowerShell-skript parallellt.</span><span class="sxs-lookup"><span data-stu-id="a8a42-117">hello only difference is that if you want tootrain from 1,000 datasets you probably want toothink of running hello following PowerShell scripts in parallel.</span></span> <span data-ttu-id="a8a42-118">Hur toodo som ligger utanför hello av den här artikeln, men du kan hitta exempel på PowerShell flertrådsteknik på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="a8a42-118">How toodo that is beyond hello scope of this article, but you can find examples of PowerShell multi-threading on hello Internet.</span></span>  
> 
> 

## <a name="set-up-hello-training-experiment"></a><span data-ttu-id="a8a42-119">Ställ in hello träningsexperiment</span><span class="sxs-lookup"><span data-stu-id="a8a42-119">Set up hello training experiment</span></span>
<span data-ttu-id="a8a42-120">Vi toouse exempel [träningsexperiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) som vi redan har skapat i hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span><span class="sxs-lookup"><span data-stu-id="a8a42-120">We're going toouse an example [training experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) that we've already created in hello [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com).</span></span> <span data-ttu-id="a8a42-121">Öppna experimentet i din [Azure Machine Learning Studio](https://studio.azureml.net) arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a8a42-121">Open this experiment in your [Azure Machine Learning Studio](https://studio.azureml.net) workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="a8a42-122">I ordning toofollow tillsammans med det här exemplet vill du kanske toouse standard arbetsytan i stället för en kostnadsfri arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="a8a42-122">In order toofollow along with this example, you may want toouse a standard workspace rather than a free workspace.</span></span> <span data-ttu-id="a8a42-123">Vi ska skapa en slutpunkt för varje kund - 10-slutpunkter – totalt och som kräver en standard arbetsyta eftersom en kostnadsfri arbetsyta är begränsat too3 slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a8a42-123">We'll be creating one endpoint for each customer - for a total of 10 endpoints - and that will require a standard workspace since a free workspace is limited too3 endpoints.</span></span> <span data-ttu-id="a8a42-124">Om du bara har en kostnadsfri arbetsyta bara ändra hello skript under tooallow för endast 3 platser.</span><span class="sxs-lookup"><span data-stu-id="a8a42-124">If you only have a free workspace, just modify hello scripts below tooallow for only 3 locations.</span></span>
> 
> 

<span data-ttu-id="a8a42-125">hello experimentet använder en **importera Data** modulen tooimport hello utbildning dataset *customer001.csv* från ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a8a42-125">hello experiment uses an **Import Data** module tooimport hello training dataset *customer001.csv* from an Azure storage account.</span></span> <span data-ttu-id="a8a42-126">Antar vi att vi har samlat in utbildning datauppsättningar från alla cykel uthyrning platser och lagrat dem i samma blob lagringsplats med filnamn som sträcker sig från hello *rentalloc001.csv* för*rentalloc10.csv* .</span><span class="sxs-lookup"><span data-stu-id="a8a42-126">Let's assume we have collected training datasets from all bike rental locations and stored them in hello same blob storage location with file names ranging from *rentalloc001.csv* too*rentalloc10.csv*.</span></span>

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/reader-module.png)

<span data-ttu-id="a8a42-128">Observera att en **Web Service utdata** modulen har lagts till toohello **Träningsmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="a8a42-128">Note that a **Web Service Output** module has been added toohello **Train Model** module.</span></span>
<span data-ttu-id="a8a42-129">När experimentet distribueras som en webbtjänst kan returneras hello slutpunkten som är associerad med den utgående hello tränade modellen i hello-format för en .ilearner-fil.</span><span class="sxs-lookup"><span data-stu-id="a8a42-129">When this experiment is deployed as a web service, hello endpoint associated with that output will return hello trained model in hello format of a .ilearner file.</span></span>

<span data-ttu-id="a8a42-130">Observera även att vi ställer in en web service-parameter för hello URL som hello **importera Data** modulen används.</span><span class="sxs-lookup"><span data-stu-id="a8a42-130">Also note that we set up a web service parameter for hello URL that hello **Import Data** module uses.</span></span> <span data-ttu-id="a8a42-131">Detta gör att vi toouse hello parametern toospecify enskilda utbildning datauppsättningar tootrain hello modellen för varje plats.</span><span class="sxs-lookup"><span data-stu-id="a8a42-131">This allows us toouse hello parameter toospecify individual training datasets tootrain hello model for each location.</span></span>
<span data-ttu-id="a8a42-132">Det finns andra sätt som vi gick har gjort det, till exempel använder en SQL-fråga med en web service parametern tooget data från en SQL Azure-databas, eller bara använda en **webbtjänst** modulen toopass i en dataset toohello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8a42-132">There are other ways we could have done this, such as using a SQL query with a web service parameter tooget data from a SQL Azure database, or simply using a  **Web Service Input** module toopass in a dataset toohello web service.</span></span>

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/web-service-output.png)

<span data-ttu-id="a8a42-134">Nu ska vi kör experimentet utbildning med hjälp av standardvärdet för hello *rental001.csv* som hello utbildning dataset.</span><span class="sxs-lookup"><span data-stu-id="a8a42-134">Now, let's run this training experiment using hello default value *rental001.csv* as hello training dataset.</span></span> <span data-ttu-id="a8a42-135">Om du visar hello utdata från hello **utvärdera** modulen (klicka på hello utdata och välj **visualisera**), du kan se vi hämta ett hyfsad prestanda för *AUC* = 0.91.</span><span class="sxs-lookup"><span data-stu-id="a8a42-135">If you view hello output of hello **Evaluate** module (click hello output and select **Visualize**), you can see we get a decent performance of *AUC* = 0.91.</span></span> <span data-ttu-id="a8a42-136">Vi är nu redo toodeploy en webbtjänst utanför utbildning experimentet.</span><span class="sxs-lookup"><span data-stu-id="a8a42-136">At this point, we're ready toodeploy a web service out of this training experiment.</span></span>

## <a name="deploy-hello-training-and-scoring-web-services"></a><span data-ttu-id="a8a42-137">Distribuera hello utbildning och bedömningen webbtjänster</span><span class="sxs-lookup"><span data-stu-id="a8a42-137">Deploy hello training and scoring web services</span></span>
<span data-ttu-id="a8a42-138">toodeploy hello utbildning webbtjänst, klicka på hello **konfigurera Web Service** nedan hello experimentet och välj **distribuera webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="a8a42-138">toodeploy hello training web service, click hello **Set Up Web Service** button below hello experiment canvas and select **Deploy Web Service**.</span></span> <span data-ttu-id="a8a42-139">Anropa den här webbtjänsten ”” cykel uthyrning utbildning ”.</span><span class="sxs-lookup"><span data-stu-id="a8a42-139">Call this web service ""Bike Rental Training".</span></span>

<span data-ttu-id="a8a42-140">Nu måste vi toodeploy hello bedömningsprofil webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8a42-140">Now we need toodeploy hello scoring web service.</span></span>
<span data-ttu-id="a8a42-141">toodo kan vi kan klicka på **konfigurera Web Service** nedan hello arbetsytan och välj **förutsägande webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="a8a42-141">toodo this, we can click **Set Up Web Service** below hello canvas and select **Predictive Web Service**.</span></span> <span data-ttu-id="a8a42-142">Detta skapar ett bedömningsprofil experiment.</span><span class="sxs-lookup"><span data-stu-id="a8a42-142">This creates a scoring experiment.</span></span>
<span data-ttu-id="a8a42-143">Vi behöver toomake några mindre justeringar toomake den fungerar som en webbtjänst som indata för att ta bort hello etikettkolumnen ”cnt” från hello och begränsa hello utdata tooonly hello instans-id och hello motsvarande förväntad värde.</span><span class="sxs-lookup"><span data-stu-id="a8a42-143">We'll need toomake a few minor adjustments toomake it work as a web service, such as removing hello label column "cnt" from hello input data and limiting hello output tooonly hello instance id and hello corresponding predicted value.</span></span>

<span data-ttu-id="a8a42-144">toosave själv som fungerar, kan du helt enkelt öppna hello [prediktivt experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) i hello galleriet som redan har förberetts.</span><span class="sxs-lookup"><span data-stu-id="a8a42-144">toosave yourself that work, you can simply open hello [predictive experiment](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) in hello Gallery that's already been prepared.</span></span>

<span data-ttu-id="a8a42-145">toodeploy hello-webbtjänsten, kör hello prediktivt experiment, och klicka sedan på hello **distribuera webbtjänsten** nedan hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="a8a42-145">toodeploy hello web service, run hello predictive experiment, then click hello **Deploy Web Service** button below hello canvas.</span></span> <span data-ttu-id="a8a42-146">Namnet hello bedömningen webbtjänst ”cykel uthyrning bedömningen” ”.</span><span class="sxs-lookup"><span data-stu-id="a8a42-146">Name hello scoring web service "Bike Rental Scoring"".</span></span>

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a><span data-ttu-id="a8a42-147">Skapa 10 slutpunkter för identiska webbtjänster med PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8a42-147">Create 10 identical web service endpoints with PowerShell</span></span>
<span data-ttu-id="a8a42-148">Den här webbtjänsten levereras med en standardslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a8a42-148">This web service comes with a default endpoint.</span></span> <span data-ttu-id="a8a42-149">Men vi har inte intresserad hello standardslutpunkten eftersom den inte uppdateras.</span><span class="sxs-lookup"><span data-stu-id="a8a42-149">But we're not as interested in hello default endpoint since it can't be updated.</span></span> <span data-ttu-id="a8a42-150">Vi behöver toodo är toocreate 10 ytterligare slutpunkter, ett för varje plats.</span><span class="sxs-lookup"><span data-stu-id="a8a42-150">What we need toodo is toocreate 10 additional endpoints, one for each location.</span></span> <span data-ttu-id="a8a42-151">Vi ska göra detta med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a8a42-151">We'll do this with PowerShell.</span></span>

<span data-ttu-id="a8a42-152">Först måste konfigurerar vi våra PowerShell-miljö:</span><span class="sxs-lookup"><span data-stu-id="a8a42-152">First, we set up our PowerShell environment:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and is properly set toopoint toohello valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

<span data-ttu-id="a8a42-153">Kör sedan hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="a8a42-153">Then, run hello following PowerShell command:</span></span>

    # Create 10 endpoints on hello scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

<span data-ttu-id="a8a42-154">Nu har vi skapat 10 slutpunkter och alla innehålla samma tränad modell tränas på hello *customer001.csv*.</span><span class="sxs-lookup"><span data-stu-id="a8a42-154">Now we've created 10 endpoints and they all contain hello same trained model trained on *customer001.csv*.</span></span> <span data-ttu-id="a8a42-155">Du kan visa dem i hello Azure-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="a8a42-155">You can view them in hello Azure Management Portal.</span></span>

![Bild](./media/machine-learning-create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-hello-endpoints-toouse-separate-training-datasets-using-powershell"></a><span data-ttu-id="a8a42-157">Uppdatera hello slutpunkter toouse separata tränings datauppsättningar med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="a8a42-157">Update hello endpoints toouse separate training datasets using PowerShell</span></span>
<span data-ttu-id="a8a42-158">hello nästa steg är tooupdate hello slutpunkter med modeller tränas unikt på varje enskild kunddata.</span><span class="sxs-lookup"><span data-stu-id="a8a42-158">hello next step is tooupdate hello endpoints with models uniquely trained on each customer's individual data.</span></span> <span data-ttu-id="a8a42-159">Men vi måste först tooproduce dessa modeller från hello **cykel uthyrning utbildning** webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8a42-159">But first we need tooproduce these models from hello **Bike Rental Training** web service.</span></span> <span data-ttu-id="a8a42-160">Vi går tillbaka toohello **cykel uthyrning utbildning** webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a8a42-160">Let's go back toohello **Bike Rental Training** web service.</span></span> <span data-ttu-id="a8a42-161">Vi behöver toocall sin BES slutpunkt 10 gånger med 10 olika utbildning datauppsättningar i ordning tooproduce 10 olika modeller.</span><span class="sxs-lookup"><span data-stu-id="a8a42-161">We need toocall its BES endpoint 10 times with 10 different training datasets in order tooproduce 10 different models.</span></span> <span data-ttu-id="a8a42-162">Vi använder hello **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet toodo detta.</span><span class="sxs-lookup"><span data-stu-id="a8a42-162">We'll use hello **InovkeAmlWebServiceBESEndpoint** PowerShell cmdlet toodo this.</span></span>

<span data-ttu-id="a8a42-163">Du måste också tooprovide autentiseringsuppgifter för blob storage-konto i `$configContent`, nämligen t.o.m. hello `AccountName`, `AccountKey` och `RelativeLocation`.</span><span class="sxs-lookup"><span data-stu-id="a8a42-163">You will also need tooprovide credentials for your blob storage account into `$configContent`, namely, at hello fields `AccountName`, `AccountKey` and `RelativeLocation`.</span></span> <span data-ttu-id="a8a42-164">Hej `AccountName` kan vara något av ditt kontonamn som visas i hello **klassiska Azure-hanteringsportalen** (*lagring* fliken).</span><span class="sxs-lookup"><span data-stu-id="a8a42-164">hello `AccountName` can be one of your account names, as seen in hello **Classic Azure Management Portal** (*Storage* tab).</span></span> <span data-ttu-id="a8a42-165">När du klickar på ett lagringskonto dess `AccountKey` hittar genom att trycka på hello **hantera åtkomstnycklar** längst hello längst ned och kopiera hello *primära åtkomstnyckeln*.</span><span class="sxs-lookup"><span data-stu-id="a8a42-165">Once you click on a storage account, its `AccountKey` can be found by pressing hello **Manage Access Keys** button at hello bottom and copying hello *Primary Access Key*.</span></span> <span data-ttu-id="a8a42-166">Hej `RelativeLocation` är hello sökväg relativa tooyour lagring där en ny modell ska lagras.</span><span class="sxs-lookup"><span data-stu-id="a8a42-166">hello `RelativeLocation` is hello path relative tooyour storage where a new model will be stored.</span></span> <span data-ttu-id="a8a42-167">Till exempel hello sökvägen `hai/retrain/bike_rental/` i hello-skriptet nedan punkter tooa behållare med namnet `hai`, och `/retrain/bike_rental/` är undermappar.</span><span class="sxs-lookup"><span data-stu-id="a8a42-167">For instance, hello path `hai/retrain/bike_rental/` in hello script below points tooa container named `hai`, and `/retrain/bike_rental/` are subfolders.</span></span> <span data-ttu-id="a8a42-168">För närvarande kan du skapa undermappar via hello portalens användargränssnitt, men det finns [flera Azure-lagringsutforskare](../storage/common/storage-explorers.md) som gör det möjligt toodo så.</span><span class="sxs-lookup"><span data-stu-id="a8a42-168">Currently, you cannot create subfolders through hello portal UI, but there are [several Azure Storage Explorers](../storage/common/storage-explorers.md) that allow you toodo so.</span></span> <span data-ttu-id="a8a42-169">Vi rekommenderar att du skapar en ny behållare i din lagring toostore hello nya tränade modeller (.ilearner-filer) på följande sätt: från lagringssidan klickar du på hello **Lägg till** knappen längst ned hello och ger den namnet `retrain`.</span><span class="sxs-lookup"><span data-stu-id="a8a42-169">It is recommended that you create a new container in your storage toostore hello new trained models (.ilearner files) as follows: from your storage page, click on hello **Add** button at hello bottom and name it `retrain`.</span></span> <span data-ttu-id="a8a42-170">Sammanfattningsvis hello nödvändiga ändringar toohello skriptet nedan gäller för`AccountName`, `AccountKey` och `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span><span class="sxs-lookup"><span data-stu-id="a8a42-170">In summary, hello necassary changes toohello script below pertain too`AccountName`, `AccountKey` and `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).</span></span>

    # Invoke hello retraining API 10 times
    # This is hello default (and hello only) endpoint on hello training web service
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
> <span data-ttu-id="a8a42-171">hello BES-slutpunkten är hello stöds endast läge för den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="a8a42-171">hello BES endpoint is hello only supported mode for this operation.</span></span> <span data-ttu-id="a8a42-172">RR kan inte användas för att framställa tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="a8a42-172">RRS cannot be used for producing trained models.</span></span>
> 
> 

<span data-ttu-id="a8a42-173">Som du ser ovan, i stället för att konstruera 10 olika BES jobbet json konfigurationsfiler, vi skapa hello sträng i stället och dynamiskt feed det toohello *jobConfigString* parametern för hello  **InvokeAmlWebServceBESEndpoint** cmdlet, eftersom det inte finns egentligen inget behov av tookeep en kopia för på disken.</span><span class="sxs-lookup"><span data-stu-id="a8a42-173">As you can see above, instead of constructing 10 different BES job configuration json files, we dynamically create hello config string instead and feed it toohello *jobConfigString* parameter of hello **InvokeAmlWebServceBESEndpoint** cmdlet, since there is really no need tookeep a copy on disk.</span></span>

<span data-ttu-id="a8a42-174">Om allt går bra efter en stund bör du se 10 .ilearner filer från *model001.ilearner* för*model010.ilearner*, i ditt Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a8a42-174">If everything goes well, after a while you should see 10 .ilearner files, from *model001.ilearner* too*model010.ilearner*, in your Azure storage account.</span></span> <span data-ttu-id="a8a42-175">Nu är klar tooupdate våra 10 bedömningen web tjänstens slutpunkter med dessa modeller med hello **korrigering AmlWebServiceEndpoint** PowerShell-cmdlet.</span><span class="sxs-lookup"><span data-stu-id="a8a42-175">Now we're ready tooupdate our 10 scoring web service endpoints with these models using hello **Patch-AmlWebServiceEndpoint** PowerShell cmdlet.</span></span> <span data-ttu-id="a8a42-176">Kom ihåg igen att vi endast korrigering hello icke-förvalt slutpunkter vi programmässigt skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a8a42-176">Remember again that we can only patch hello non-default endpoints we programmatically created earlier.</span></span>

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

<span data-ttu-id="a8a42-177">Detta bör köra ganska snabbt.</span><span class="sxs-lookup"><span data-stu-id="a8a42-177">This should run fairly quickly.</span></span> <span data-ttu-id="a8a42-178">När hello körningen har slutförts ska vi har skapat 10 slutpunkter för förutsägande webbtjänster, som innehåller en tränad modell tränas unikt på hello dataset specifika tooa uthyrning plats, allt från en enda träningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="a8a42-178">When hello execution finishes, we'll have successfully created 10 predictive web service endpoints, each containing a trained model uniquely trained on hello dataset specific tooa rental location, all from a single training experiment.</span></span> <span data-ttu-id="a8a42-179">tooverify, kan du anropa dessa slutpunkter som använder hello **InvokeAmlWebServiceRRSEndpoint** cmdlet, vilket ger dem med hello samma indata och förväntat toosee olika förutsägelse resultat eftersom hello modeller tränas med olika utbildning uppsättningar.</span><span class="sxs-lookup"><span data-stu-id="a8a42-179">tooverify this, you can try calling these endpoints using hello **InvokeAmlWebServiceRRSEndpoint** cmdlet, providing them with hello same input data, and you should expect toosee different prediction results since hello models are trained with different training sets.</span></span>

## <a name="full-powershell-script"></a><span data-ttu-id="a8a42-180">Fullständig PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="a8a42-180">Full PowerShell script</span></span>
<span data-ttu-id="a8a42-181">Här är hello lista över hello fullständig källkoden:</span><span class="sxs-lookup"><span data-stu-id="a8a42-181">Here's hello listing of hello full source code:</span></span>

    Import-Module .\AzureMLPS.dll
    # Assume hello default configuration file exists and properly set toopoint toohello valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on hello scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke hello retraining API 10 times tooproduce 10 regression models in .ilearner format
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

    # Patch hello 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
