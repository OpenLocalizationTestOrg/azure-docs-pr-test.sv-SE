---
title: 'Steg 2: Ladda upp data till en Machine Learning-experiment | Microsoft Docs'
description: "Steg 2 av hello utveckla en förutsägelselösning genomgång: Överför lagras offentliga data i Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 0ea21dcca2d0934ed06508560cf85cf31b48ce6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a><span data-ttu-id="7bb0e-103">Genomgång steg 2: Överför befintliga data i ett Azure Machine Learning-experiment</span><span class="sxs-lookup"><span data-stu-id="7bb0e-103">Walkthrough Step 2: Upload existing data into an Azure Machine Learning experiment</span></span>
<span data-ttu-id="7bb0e-104">Detta är hello andra steget i hello genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="7bb0e-104">This is hello second step of hello walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="7bb0e-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="7bb0e-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. <span data-ttu-id="7bb0e-106">**Överför befintliga data**</span><span class="sxs-lookup"><span data-stu-id="7bb0e-106">**Upload existing data**</span></span>
3. [<span data-ttu-id="7bb0e-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="7bb0e-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="7bb0e-108">Träna och utvärdera hello modeller</span><span class="sxs-lookup"><span data-stu-id="7bb0e-108">Train and evaluate hello models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="7bb0e-109">Distribuera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7bb0e-109">Deploy hello Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="7bb0e-110">Få åtkomst till hello-webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="7bb0e-110">Access hello Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="7bb0e-111">toodevelop en förutsägelsemodell för kreditrisk vi behöver data att vi kan använda tootrain och testa hello modellen.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-111">toodevelop a predictive model for credit risk, we need data that we can use tootrain and then test hello model.</span></span> <span data-ttu-id="7bb0e-112">Den här genomgången använder vi hello ”UCI Statlog (tyska kredit Data) datauppsättning” från hello UC Irvine Machine Learning-databasen.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-112">For this walkthrough, we'll use hello "UCI Statlog (German Credit Data) Data Set" from hello UC Irvine Machine Learning repository.</span></span> <span data-ttu-id="7bb0e-113">Du hittar den här:</span><span class="sxs-lookup"><span data-stu-id="7bb0e-113">You can find it here:</span></span>  
<span data-ttu-id="7bb0e-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+Credit+data)</a></span><span class="sxs-lookup"><span data-stu-id="7bb0e-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a></span></span>

<span data-ttu-id="7bb0e-115">Vi använder hello-fil med namnet **german.data**.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-115">We'll use hello file named **german.data**.</span></span> <span data-ttu-id="7bb0e-116">Hämta den här filen tooyour lokala hårddisken.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-116">Download this file tooyour local hard drive.</span></span>  

<span data-ttu-id="7bb0e-117">Hej **german.data** datamängden innehåller rader med 20 variabler för 1000 senaste ansöker om kredit.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-117">hello **german.data** dataset contains rows of 20 variables for 1000 past applicants for credit.</span></span> <span data-ttu-id="7bb0e-118">Variablerna 20 representerar hello dataset uppsättning funktioner (hello *funktionen vector*), vilket möjliggör identifieringsegenskaper för varje kredit sökanden.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-118">These 20 variables represent hello dataset's set of features (hello *feature vector*), which provides identifying characteristics for each credit applicant.</span></span> <span data-ttu-id="7bb0e-119">Ytterligare en kolumn i varje rad representerar hello sökandens beräknade kreditrisk med 700 sökanden identifieras som en låg kreditrisk och 300 som en hög risk.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-119">An additional column in each row represents hello applicant's calculated credit risk, with 700 applicants identified as a low credit risk and 300 as a high risk.</span></span>

<span data-ttu-id="7bb0e-120">hello UCI webbplatser som innehåller en beskrivning av hello attribut för hello funktionen-vektor för dessa data.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-120">hello UCI website provides a description of hello attributes of hello feature vector for this data.</span></span> <span data-ttu-id="7bb0e-121">Detta inkluderar finansiell information, kredithistorik, anställningsstatus och personlig information.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-121">This includes financial information, credit history, employment status, and personal information.</span></span> <span data-ttu-id="7bb0e-122">För varje ansökan har en binär klassificering angivna som anger om de är låg eller hög kredit risk.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-122">For each applicant, a binary rating has been given indicating whether they are a low or high credit risk.</span></span> 

<span data-ttu-id="7bb0e-123">Vi använder den här data tootrain en förutsägelseanalysmodell.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-123">We'll use this data tootrain a predictive analytics model.</span></span> <span data-ttu-id="7bb0e-124">När det är klart bör vår modell kunna tooaccept en funktionen-vektor för en ny person och förutsäga om han eller hon är en låg eller hög kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-124">When we're done, our model should be able tooaccept a feature vector for a new individual and predict whether he or she is a low or high credit risk.</span></span>  

<span data-ttu-id="7bb0e-125">Här är en intressant snodd.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-125">Here's an interesting twist.</span></span> <span data-ttu-id="7bb0e-126">Hej beskrivning av hello dataset i hello UCI webbplats nämns vad det kostar om vi misclassify kreditrisken för en person.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-126">hello description of hello dataset on hello UCI website mentions what it costs if we misclassify a person's credit risk.</span></span>
<span data-ttu-id="7bb0e-127">Om hello modellen beräknar en hög kreditrisken för en person som är faktiskt en låg kreditrisk, har hello modellen gjort en felklassificering.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-127">If hello model predicts a high credit risk for someone who is actually a low credit risk, hello model has made a misclassification.</span></span>
<span data-ttu-id="7bb0e-128">Men hello omvänd felklassificering är fem gånger dyraste toohello finansinstitut: om hello modellen beräknar en låg kreditrisken för en person som är faktiskt en hög kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-128">But hello reverse misclassification is five times more costly toohello financial institution: if hello model predicts a low credit risk for someone who is actually a high credit risk.</span></span>

<span data-ttu-id="7bb0e-129">Så vill vi tootrain vår modell så att hello kostnaden för den här senare typen av felklassificering är fem gånger större än misclassifying hello andra sätt.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-129">So, we want tootrain our model so that hello cost of this latter type of misclassification is five times higher than misclassifying hello other way.</span></span>
<span data-ttu-id="7bb0e-130">Ett enkelt sätt toodo detta när hello modell i vårt experiment är genom att duplicera (fem gånger) transaktioner som representerar någon med en hög kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-130">One simple way toodo this when training hello model in our experiment is by duplicating (five times) those entries that represent someone with a high credit risk.</span></span> <span data-ttu-id="7bb0e-131">Sedan om hello modellen felaktigt någon som en låg kreditrisk klassificerar när de är faktiskt en hög risk, har hello modellen den samma felklassificering fem gånger en gång för varje kopia.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-131">Then, if hello model misclassifies someone as a low credit risk when they're actually a high risk, hello model does that same misclassification five times, once for each duplicate.</span></span> <span data-ttu-id="7bb0e-132">Detta ökar hello kostnaden för det här felet i hello utbildning resultat.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-132">This will increase hello cost of this error in hello training results.</span></span>


## <a name="convert-hello-dataset-format"></a><span data-ttu-id="7bb0e-133">Konvertera hello dataset-format</span><span class="sxs-lookup"><span data-stu-id="7bb0e-133">Convert hello dataset format</span></span>
<span data-ttu-id="7bb0e-134">hello ursprungliga datauppsättningen används ett tomt kommaavgränsade format.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-134">hello original dataset uses a blank-separated format.</span></span> <span data-ttu-id="7bb0e-135">Machine Learning Studio fungerar bättre med en fil med kommaavgränsade värden (CSV), så att det kommer att konvertera hello datauppsättningen genom att ersätta blanksteg med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-135">Machine Learning Studio works better with a comma-separated value (CSV) file, so we'll convert hello dataset by replacing spaces with commas.</span></span>  

<span data-ttu-id="7bb0e-136">Det finns många sätt tooconvert dessa data.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-136">There are many ways tooconvert this data.</span></span> <span data-ttu-id="7bb0e-137">Ett sätt är med hjälp av hello följande Windows PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="7bb0e-137">One way is by using hello following Windows PowerShell command:</span></span>   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

<span data-ttu-id="7bb0e-138">Ett annat sätt är med hello Unix sed kommando:</span><span class="sxs-lookup"><span data-stu-id="7bb0e-138">Another way is by using hello Unix sed command:</span></span>  

    sed 's/ /,/g' german.data > german.csv  

<span data-ttu-id="7bb0e-139">I båda fallen kan vi har skapat en CSV-version av hello data i en fil med namnet **german.csv** som vi kan använda i vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-139">In either case, we have created a comma-separated version of hello data in a file named **german.csv** that we can use in our experiment.</span></span>

## <a name="upload-hello-dataset-toomachine-learning-studio"></a><span data-ttu-id="7bb0e-140">Överför hello dataset tooMachine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="7bb0e-140">Upload hello dataset tooMachine Learning Studio</span></span>
<span data-ttu-id="7bb0e-141">När hello data har konverterade tooCSV format, behöver vi tooupload till Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-141">Once hello data has been converted tooCSV format, we need tooupload it into Machine Learning Studio.</span></span> 

1. <span data-ttu-id="7bb0e-142">Öppna hello Machine Learning Studio-startsidan ([https://studio.azureml.net](https://studio.azureml.net)).</span><span class="sxs-lookup"><span data-stu-id="7bb0e-142">Open hello Machine Learning Studio home page ([https://studio.azureml.net](https://studio.azureml.net)).</span></span> 

2. <span data-ttu-id="7bb0e-143">Klicka på hello ![menyn][1] i hello övre vänstra hörnet av hello klickar du på **Azure Machine Learning**väljer **Studio**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-143">Click hello menu ![Menu][1] in hello upper-left corner of hello window, click **Azure Machine Learning**, select **Studio**, and sign in.</span></span>

3. <span data-ttu-id="7bb0e-144">Klicka på **+ ny** längst hello hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-144">Click **+NEW** at hello bottom of hello window.</span></span>

4. <span data-ttu-id="7bb0e-145">Välj **DATASET**.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-145">Select **DATASET**.</span></span>

5. <span data-ttu-id="7bb0e-146">Välj **från en lokal fil**.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-146">Select **FROM LOCAL FILE**.</span></span>

    ![Lägg till en datamängd från en lokal fil][2]

6. <span data-ttu-id="7bb0e-148">I hello **ladda upp en ny datamängd** dialogrutan klickar du på **Bläddra** och hitta hello **german.csv** fil som du skapade.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-148">In hello **Upload a new dataset** dialog, click **Browse** and find hello **german.csv** file you created.</span></span>

7. <span data-ttu-id="7bb0e-149">Ange ett namn för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-149">Enter a name for hello dataset.</span></span> <span data-ttu-id="7bb0e-150">Anropa den ”UCI tyska kreditkortdata” i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-150">For this walkthrough, call it "UCI German Credit Card Data".</span></span>

8. <span data-ttu-id="7bb0e-151">Datatypen, Välj **generiska CSV-filen utan rubrik (. nh.csv)**.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-151">For data type, select **Generic CSV File With no header (.nh.csv)**.</span></span>

9. <span data-ttu-id="7bb0e-152">Lägg till en beskrivning om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-152">Add a description if you’d like.</span></span>

10. <span data-ttu-id="7bb0e-153">Klicka på hello **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-153">Click hello **OK** check mark.</span></span>  

    ![Överför hello dataset][3]

<span data-ttu-id="7bb0e-155">Detta filöverföringar hello data i en dataset-modul som vi kan använda i ett experiment.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-155">This uploads hello data into a dataset module that we can use in an experiment.</span></span>

<span data-ttu-id="7bb0e-156">Du kan hantera datauppsättningar som du har överfört tooStudio genom att klicka på hello **DATAUPPSÄTTNINGAR** fliken toohello vänsterkant hello Studio-fönstret.</span><span class="sxs-lookup"><span data-stu-id="7bb0e-156">You can manage datasets that you've uploaded tooStudio by clicking hello **DATASETS** tab toohello left of hello Studio window.</span></span>

![Hantera datamängder][4]

<span data-ttu-id="7bb0e-158">Mer information om hur du importerar andra typer av data i ett experiment finns [importera utbildningsdata till Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span><span class="sxs-lookup"><span data-stu-id="7bb0e-158">For more information about importing other types of data into an experiment, see [Import your training data into Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span></span>

<span data-ttu-id="7bb0e-159">**Nästa: [skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span><span class="sxs-lookup"><span data-stu-id="7bb0e-159">**Next: [Create a new experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span></span>

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
