---
title: 'Steg 2: Ladda upp data till en Machine Learning-experiment | Microsoft Docs'
description: "Steg 2 av utveckla en förutsägelselösning genomgång: Överför lagras offentliga data i Azure Machine Learning Studio."
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
ms.openlocfilehash: c2ab5f5252e1ea1ec51f6c3bd489826c70ff011c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a><span data-ttu-id="20476-103">Genomgång steg 2: Överför befintliga data i ett Azure Machine Learning-experiment</span><span class="sxs-lookup"><span data-stu-id="20476-103">Walkthrough Step 2: Upload existing data into an Azure Machine Learning experiment</span></span>
<span data-ttu-id="20476-104">Detta är det andra steget i den här genomgången [utveckla en förutsägelseanalys i Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span><span class="sxs-lookup"><span data-stu-id="20476-104">This is the second step of the walkthrough, [Develop a predictive analytics solution in Azure Machine Learning](machine-learning-walkthrough-develop-predictive-solution.md)</span></span>

1. [<span data-ttu-id="20476-105">Skapa en Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="20476-105">Create a Machine Learning workspace</span></span>](machine-learning-walkthrough-1-create-ml-workspace.md)
2. <span data-ttu-id="20476-106">**Överför befintliga data**</span><span class="sxs-lookup"><span data-stu-id="20476-106">**Upload existing data**</span></span>
3. [<span data-ttu-id="20476-107">Skapa ett nytt experiment</span><span class="sxs-lookup"><span data-stu-id="20476-107">Create a new experiment</span></span>](machine-learning-walkthrough-3-create-new-experiment.md)
4. [<span data-ttu-id="20476-108">Träna och utvärdera modellerna</span><span class="sxs-lookup"><span data-stu-id="20476-108">Train and evaluate the models</span></span>](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [<span data-ttu-id="20476-109">Distribuera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="20476-109">Deploy the Web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
6. [<span data-ttu-id="20476-110">Få åtkomst till webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="20476-110">Access the Web service</span></span>](machine-learning-walkthrough-6-access-web-service.md)

- - -
<span data-ttu-id="20476-111">För att utveckla en förutsägelsemodell för kreditrisk behöver vi data som vi kan använda för att träna och testa modellen.</span><span class="sxs-lookup"><span data-stu-id="20476-111">To develop a predictive model for credit risk, we need data that we can use to train and then test the model.</span></span> <span data-ttu-id="20476-112">Den här genomgången använder vi ”UCI Statlog (tyska kredit Data) datauppsättningen” från UC Irvine Machine Learning-databasen.</span><span class="sxs-lookup"><span data-stu-id="20476-112">For this walkthrough, we'll use the "UCI Statlog (German Credit Data) Data Set" from the UC Irvine Machine Learning repository.</span></span> <span data-ttu-id="20476-113">Du hittar den här:</span><span class="sxs-lookup"><span data-stu-id="20476-113">You can find it here:</span></span>  
<span data-ttu-id="20476-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://Archive.ICS.uci.edu/ml/DataSets/Statlog+(German+Credit+data)</a></span><span class="sxs-lookup"><span data-stu-id="20476-114"><a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a></span></span>

<span data-ttu-id="20476-115">Vi använder den fil som heter **german.data**.</span><span class="sxs-lookup"><span data-stu-id="20476-115">We'll use the file named **german.data**.</span></span> <span data-ttu-id="20476-116">Hämta den här filen till den lokala hårddisken.</span><span class="sxs-lookup"><span data-stu-id="20476-116">Download this file to your local hard drive.</span></span>  

<span data-ttu-id="20476-117">Den **german.data** datamängden innehåller rader med 20 variabler för 1000 senaste ansöker om kredit.</span><span class="sxs-lookup"><span data-stu-id="20476-117">The **german.data** dataset contains rows of 20 variables for 1000 past applicants for credit.</span></span> <span data-ttu-id="20476-118">Variablerna 20 representerar den dataset uppsättning funktioner (den *funktionen vector*), vilket möjliggör identifieringsegenskaper för varje kredit sökanden.</span><span class="sxs-lookup"><span data-stu-id="20476-118">These 20 variables represent the dataset's set of features (the *feature vector*), which provides identifying characteristics for each credit applicant.</span></span> <span data-ttu-id="20476-119">Ytterligare en kolumn i varje rad representerar sökandens beräknade kreditrisk med 700 sökanden identifieras som en låg kreditrisk och 300 som en hög risk.</span><span class="sxs-lookup"><span data-stu-id="20476-119">An additional column in each row represents the applicant's calculated credit risk, with 700 applicants identified as a low credit risk and 300 as a high risk.</span></span>

<span data-ttu-id="20476-120">UCI webbplatser som innehåller en beskrivning av attribut för funktionen vektorn för dessa data.</span><span class="sxs-lookup"><span data-stu-id="20476-120">The UCI website provides a description of the attributes of the feature vector for this data.</span></span> <span data-ttu-id="20476-121">Detta inkluderar finansiell information, kredithistorik, anställningsstatus och personlig information.</span><span class="sxs-lookup"><span data-stu-id="20476-121">This includes financial information, credit history, employment status, and personal information.</span></span> <span data-ttu-id="20476-122">För varje ansökan har en binär klassificering angivna som anger om de är låg eller hög kredit risk.</span><span class="sxs-lookup"><span data-stu-id="20476-122">For each applicant, a binary rating has been given indicating whether they are a low or high credit risk.</span></span> 

<span data-ttu-id="20476-123">Vi använder informationen för att träna en förutsägelseanalysmodell.</span><span class="sxs-lookup"><span data-stu-id="20476-123">We'll use this data to train a predictive analytics model.</span></span> <span data-ttu-id="20476-124">När det är klart ska vår modell kunna acceptera en funktionen-vektor för en ny person och förutsäga om han eller hon är en låg eller hög kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="20476-124">When we're done, our model should be able to accept a feature vector for a new individual and predict whether he or she is a low or high credit risk.</span></span>  

<span data-ttu-id="20476-125">Här är en intressant snodd.</span><span class="sxs-lookup"><span data-stu-id="20476-125">Here's an interesting twist.</span></span> <span data-ttu-id="20476-126">Beskrivning av datamängden på webbplatsen UCI nämns vad det kostar om vi misclassify kreditrisken för en person.</span><span class="sxs-lookup"><span data-stu-id="20476-126">The description of the dataset on the UCI website mentions what it costs if we misclassify a person's credit risk.</span></span>
<span data-ttu-id="20476-127">Om modellen beräknar en hög kreditrisken för en person som är faktiskt en låg kreditrisk, har modellen gjort en felklassificering.</span><span class="sxs-lookup"><span data-stu-id="20476-127">If the model predicts a high credit risk for someone who is actually a low credit risk, the model has made a misclassification.</span></span>
<span data-ttu-id="20476-128">Men omvänd felklassificering är fem gånger dyrare att finansinstitutet: om modellen beräknar en låg kreditrisken för en person som är faktiskt en hög kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="20476-128">But the reverse misclassification is five times more costly to the financial institution: if the model predicts a low credit risk for someone who is actually a high credit risk.</span></span>

<span data-ttu-id="20476-129">Därför vill vi träna vår modell så att kostnaden för den här senare typen av felklassificering är fem gånger större än misclassifying på andra sätt.</span><span class="sxs-lookup"><span data-stu-id="20476-129">So, we want to train our model so that the cost of this latter type of misclassification is five times higher than misclassifying the other way.</span></span>
<span data-ttu-id="20476-130">Det är ett enkelt sätt att göra detta när träna modellen i vårt experiment genom att duplicera (fem gånger) transaktioner som representerar någon med en hög kreditrisk.</span><span class="sxs-lookup"><span data-stu-id="20476-130">One simple way to do this when training the model in our experiment is by duplicating (five times) those entries that represent someone with a high credit risk.</span></span> <span data-ttu-id="20476-131">Sedan om modellen felaktigt någon som en låg kreditrisk klassificerar när de är faktiskt en hög risk, har modellen den samma felklassificering fem gånger, en gång för varje kopia.</span><span class="sxs-lookup"><span data-stu-id="20476-131">Then, if the model misclassifies someone as a low credit risk when they're actually a high risk, the model does that same misclassification five times, once for each duplicate.</span></span> <span data-ttu-id="20476-132">Detta ökar kostnaden för det här felet i utbildning resultaten.</span><span class="sxs-lookup"><span data-stu-id="20476-132">This will increase the cost of this error in the training results.</span></span>


## <a name="convert-the-dataset-format"></a><span data-ttu-id="20476-133">Konvertera dataset-format</span><span class="sxs-lookup"><span data-stu-id="20476-133">Convert the dataset format</span></span>
<span data-ttu-id="20476-134">Den ursprungliga datauppsättningen används ett tomt kommaavgränsade format.</span><span class="sxs-lookup"><span data-stu-id="20476-134">The original dataset uses a blank-separated format.</span></span> <span data-ttu-id="20476-135">Machine Learning Studio fungerar bättre med en fil med kommaavgränsade värden (CSV), så att det kommer att konvertera datauppsättningen genom att ersätta blanksteg med kommatecken.</span><span class="sxs-lookup"><span data-stu-id="20476-135">Machine Learning Studio works better with a comma-separated value (CSV) file, so we'll convert the dataset by replacing spaces with commas.</span></span>  

<span data-ttu-id="20476-136">Det finns många sätt att omvandla data.</span><span class="sxs-lookup"><span data-stu-id="20476-136">There are many ways to convert this data.</span></span> <span data-ttu-id="20476-137">Ett sätt är med hjälp av följande Windows PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="20476-137">One way is by using the following Windows PowerShell command:</span></span>   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

<span data-ttu-id="20476-138">Ett annat sätt är med hjälp av kommandot sed Unix:</span><span class="sxs-lookup"><span data-stu-id="20476-138">Another way is by using the Unix sed command:</span></span>  

    sed 's/ /,/g' german.data > german.csv  

<span data-ttu-id="20476-139">I båda fallen kan vi har skapat en CSV-version av data i en fil med namnet **german.csv** som vi kan använda i vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="20476-139">In either case, we have created a comma-separated version of the data in a file named **german.csv** that we can use in our experiment.</span></span>

## <a name="upload-the-dataset-to-machine-learning-studio"></a><span data-ttu-id="20476-140">Ladda upp datauppsättningen till Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="20476-140">Upload the dataset to Machine Learning Studio</span></span>
<span data-ttu-id="20476-141">När data har konverterats till CSV-format, som vi behöver överföra den till Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="20476-141">Once the data has been converted to CSV format, we need to upload it into Machine Learning Studio.</span></span> 

1. <span data-ttu-id="20476-142">Öppna startsidan för Machine Learning Studio ([https://studio.azureml.net](https://studio.azureml.net)).</span><span class="sxs-lookup"><span data-stu-id="20476-142">Open the Machine Learning Studio home page ([https://studio.azureml.net](https://studio.azureml.net)).</span></span> 

2. <span data-ttu-id="20476-143">Klicka på menyn ![menyn][1] i det övre vänstra hörnet i fönstret klickar du på **Azure Machine Learning**väljer **Studio**, och logga in.</span><span class="sxs-lookup"><span data-stu-id="20476-143">Click the menu ![Menu][1] in the upper-left corner of the window, click **Azure Machine Learning**, select **Studio**, and sign in.</span></span>

3. <span data-ttu-id="20476-144">Klicka på **+ ny** längst ned i fönstret.</span><span class="sxs-lookup"><span data-stu-id="20476-144">Click **+NEW** at the bottom of the window.</span></span>

4. <span data-ttu-id="20476-145">Välj **DATASET**.</span><span class="sxs-lookup"><span data-stu-id="20476-145">Select **DATASET**.</span></span>

5. <span data-ttu-id="20476-146">Välj **från en lokal fil**.</span><span class="sxs-lookup"><span data-stu-id="20476-146">Select **FROM LOCAL FILE**.</span></span>

    ![Lägg till en datamängd från en lokal fil][2]

6. <span data-ttu-id="20476-148">I den **ladda upp en ny datamängd** dialogrutan klickar du på **Bläddra** och Sök efter den **german.csv** fil som du skapade.</span><span class="sxs-lookup"><span data-stu-id="20476-148">In the **Upload a new dataset** dialog, click **Browse** and find the **german.csv** file you created.</span></span>

7. <span data-ttu-id="20476-149">Ange ett namn för datamängden.</span><span class="sxs-lookup"><span data-stu-id="20476-149">Enter a name for the dataset.</span></span> <span data-ttu-id="20476-150">Anropa den ”UCI tyska kreditkortdata” i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="20476-150">For this walkthrough, call it "UCI German Credit Card Data".</span></span>

8. <span data-ttu-id="20476-151">Datatypen, Välj **generiska CSV-filen utan rubrik (. nh.csv)**.</span><span class="sxs-lookup"><span data-stu-id="20476-151">For data type, select **Generic CSV File With no header (.nh.csv)**.</span></span>

9. <span data-ttu-id="20476-152">Lägg till en beskrivning om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="20476-152">Add a description if you’d like.</span></span>

10. <span data-ttu-id="20476-153">Klicka på den **OK** är markerat.</span><span class="sxs-lookup"><span data-stu-id="20476-153">Click the **OK** check mark.</span></span>  

    ![Ladda upp datauppsättningen][3]

<span data-ttu-id="20476-155">Detta överför data i en dataset-modul som vi kan använda i ett experiment.</span><span class="sxs-lookup"><span data-stu-id="20476-155">This uploads the data into a dataset module that we can use in an experiment.</span></span>

<span data-ttu-id="20476-156">Du kan hantera datauppsättningar som du har överfört till Studio genom att klicka på den **DATAUPPSÄTTNINGAR** fliken till vänster i fönstret Studio.</span><span class="sxs-lookup"><span data-stu-id="20476-156">You can manage datasets that you've uploaded to Studio by clicking the **DATASETS** tab to the left of the Studio window.</span></span>

![Hantera datamängder][4]

<span data-ttu-id="20476-158">Mer information om hur du importerar andra typer av data i ett experiment finns [importera utbildningsdata till Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span><span class="sxs-lookup"><span data-stu-id="20476-158">For more information about importing other types of data into an experiment, see [Import your training data into Azure Machine Learning Studio](machine-learning-data-science-import-data.md).</span></span>

<span data-ttu-id="20476-159">**Nästa: [skapa ett nytt experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span><span class="sxs-lookup"><span data-stu-id="20476-159">**Next: [Create a new experiment](machine-learning-walkthrough-3-create-new-experiment.md)**</span></span>

[1]: media/machine-learning-walkthrough-2-upload-data/menu.png
[2]: media/machine-learning-walkthrough-2-upload-data/add-dataset.png
[3]: media/machine-learning-walkthrough-2-upload-data/upload-dataset.png
[4]: media/machine-learning-walkthrough-2-upload-data/dataset-list.png
