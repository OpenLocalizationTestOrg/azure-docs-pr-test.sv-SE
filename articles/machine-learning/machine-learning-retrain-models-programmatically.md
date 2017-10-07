---
title: aaaRetrain Machine Learning-modeller via programmering | Microsoft Docs
description: "Lär dig hur tooprogrammatically träna om en modell och uppdatera hello web service toouse hello nyligen trained model i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: 7ae4f977-e6bf-4d04-9dde-28a66ce7b664
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl;garye;v-donglo
ms.openlocfilehash: edbb64c08f7d9edf3c76e23e0cc7e14c0125d697
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a><span data-ttu-id="e641a-103">Omtrimning av Azure Machine Learning-modeller via programmering</span><span class="sxs-lookup"><span data-stu-id="e641a-103">Retrain Machine Learning models programmatically</span></span>
<span data-ttu-id="e641a-104">I den här genomgången får du lära dig hur tooprogrammatically träna om en Azure Machine Learning-webbtjänst med hjälp av C# och hello Machine Learning Batch Execution service.</span><span class="sxs-lookup"><span data-stu-id="e641a-104">In this walkthrough, you will learn how tooprogrammatically retrain an Azure Machine Learning Web Service using C# and hello Machine Learning Batch Execution service.</span></span>

<span data-ttu-id="e641a-105">När du har retrained hello modellen visar hello följande genomgångar av hur tooupdate hello modell i förutsägande webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="e641a-105">Once you have retrained hello model, hello following walkthroughs show how tooupdate hello model in your predictive web service:</span></span>

* <span data-ttu-id="e641a-106">Om du har distribuerat en klassisk webbtjänst i hello Machine Learning-webbtjänster portal finns [träna om en klassisk webbtjänst](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-106">If you deployed a Classic web service in hello Machine Learning Web Services portal, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span> 
* <span data-ttu-id="e641a-107">Om du har distribuerat en ny webbtjänst finns [träna om en ny webbtjänst med Machine Learning Management-cmdlets för hello](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-107">If you deployed a New web service, see [Retrain a New web service using hello Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="e641a-108">En översikt över hello omtränings processen, se [träna om en Maskininlärningsmodell](machine-learning-retrain-machine-learning-model.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-108">For an overview of hello retraining process, see [Retrain a Machine Learning Model](machine-learning-retrain-machine-learning-model.md).</span></span>

<span data-ttu-id="e641a-109">Om du vill toostart med din befintliga nya Azure Resource Manager baserad webbtjänst, se [träna om en befintlig förutsägande webbtjänst](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-109">If you want toostart with your existing New Azure Resource Manager based web service, see [Retrain an existing Predictive web service](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span></span>

## <a name="create-a-training-experiment"></a><span data-ttu-id="e641a-110">Skapa ett experiment utbildning</span><span class="sxs-lookup"><span data-stu-id="e641a-110">Create a training experiment</span></span>
<span data-ttu-id="e641a-111">För det här exemplet använder du ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” hello Microsoft Azure Machine Learning prover.</span><span class="sxs-lookup"><span data-stu-id="e641a-111">For this example, you will use "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" from hello Microsoft Azure Machine Learning samples.</span></span> 

<span data-ttu-id="e641a-112">toocreate hello experiment:</span><span class="sxs-lookup"><span data-stu-id="e641a-112">toocreate hello experiment:</span></span>

1. <span data-ttu-id="e641a-113">Logga in på tooMicrosoft Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="e641a-113">Sign into tooMicrosoft Azure Machine Learning Studio.</span></span> 
2. <span data-ttu-id="e641a-114">Klicka på hello nedre högra hörnet av hello instrumentpanelen, **ny**.</span><span class="sxs-lookup"><span data-stu-id="e641a-114">On hello bottom right corner of hello dashboard, click **New**.</span></span>
3. <span data-ttu-id="e641a-115">Hello Microsoft Samples, Välj exempel 5.</span><span class="sxs-lookup"><span data-stu-id="e641a-115">From hello Microsoft Samples, select Sample 5.</span></span>
4. <span data-ttu-id="e641a-116">toorename hello experiment hello överst i hello experimentet väljer hello experiment namnet ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset”.</span><span class="sxs-lookup"><span data-stu-id="e641a-116">toorename hello experiment, at hello top of hello experiment canvas, select hello experiment name "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset".</span></span>
5. <span data-ttu-id="e641a-117">Typ av inventering modell.</span><span class="sxs-lookup"><span data-stu-id="e641a-117">Type Census Model.</span></span>
6. <span data-ttu-id="e641a-118">Hello längst ned på hello experimentet klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="e641a-118">At hello bottom of hello experiment canvas, click **Run**.</span></span>
7. <span data-ttu-id="e641a-119">Klicka på **Konfigurera webbtjänsten** och välj **Omtränings webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="e641a-119">Click **Set Up web service** and select **Retraining web service**.</span></span> 

<span data-ttu-id="e641a-120">hello följande visar hello första försöket.</span><span class="sxs-lookup"><span data-stu-id="e641a-120">hello following shows hello initial experiment.</span></span>
   
   ![Första försöket.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a><span data-ttu-id="e641a-122">Skapa en prediktivt experiment och publicera som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e641a-122">Create a predictive experiment and publish as a web service</span></span>
<span data-ttu-id="e641a-123">Nu kan du skapa ett Predicative Experiment.</span><span class="sxs-lookup"><span data-stu-id="e641a-123">Next you create a Predicative Experiment.</span></span>

1. <span data-ttu-id="e641a-124">Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="e641a-124">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Predictive Web Service**.</span></span> <span data-ttu-id="e641a-125">Detta sparar hello modellen som en Tränad modell och lägger till web service ingående och utgående moduler.</span><span class="sxs-lookup"><span data-stu-id="e641a-125">This saves hello model as a Trained Model and adds web service Input and Output modules.</span></span> 
2. <span data-ttu-id="e641a-126">Klicka på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="e641a-126">Click **Run**.</span></span> 
3. <span data-ttu-id="e641a-127">När hello experimentet har slutförts klickar du på **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="e641a-127">After hello experiment has finished running, click **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span>

> [!NOTE] 
> <span data-ttu-id="e641a-128">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="e641a-128">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="e641a-129">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-129">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

## <a name="deploy-hello-training-experiment-as-a-training-web-service"></a><span data-ttu-id="e641a-130">Distribuera hello träningsexperiment som en webbtjänst för utbildning</span><span class="sxs-lookup"><span data-stu-id="e641a-130">Deploy hello training experiment as a Training web service</span></span>
<span data-ttu-id="e641a-131">tooretrain hello tränad modell, måste du distribuera hello träningsexperiment som du har skapat som en webbtjänst för Retraining.</span><span class="sxs-lookup"><span data-stu-id="e641a-131">tooretrain hello trained model, you must deploy hello training experiment that you created as a Retraining web service.</span></span> <span data-ttu-id="e641a-132">Den här webbtjänsten måste en *Web Service utdata* modulen anslutna toohello * [Träningsmodell] [ train-model] * modul, toobe kan tooproduce nya tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="e641a-132">This web service needs a *Web Service Output* module connected toohello *[Train Model][train-model]* module, toobe able tooproduce new trained models.</span></span>

1. <span data-ttu-id="e641a-133">tooreturn toohello utbildning experiment hello experiment ikonen hello vänster Klicka på hello experiment med namnet inventering modellen.</span><span class="sxs-lookup"><span data-stu-id="e641a-133">tooreturn toohello training experiment, click hello Experiments icon in hello left pane, then click hello experiment named Census Model.</span></span>  
2. <span data-ttu-id="e641a-134">Skriv webbtjänsten i hello Experiment sökobjekt sökrutan.</span><span class="sxs-lookup"><span data-stu-id="e641a-134">In hello Search Experiment Items search box, type web service.</span></span> 
3. <span data-ttu-id="e641a-135">Dra en *webbtjänst* till hello experimentera arbetsytan och koppla dess utdata toohello *Rensa Data som saknas* modul.</span><span class="sxs-lookup"><span data-stu-id="e641a-135">Drag a *Web Service Input* module onto hello experiment canvas and connect its output toohello *Clean Missing Data* module.</span></span>  <span data-ttu-id="e641a-136">Detta säkerställer att din omtränings data bearbetas hello samma sätt som den ursprungliga informationen för utbildning.</span><span class="sxs-lookup"><span data-stu-id="e641a-136">This ensures that your retraining data is processed hello same way as your original training data.</span></span>
4. <span data-ttu-id="e641a-137">Dra två *webbtjänsten utdata* moduler på hello experimentera arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="e641a-137">Drag two *web service Output* modules onto hello experiment canvas.</span></span> <span data-ttu-id="e641a-138">Ansluta hello utdata från hello *Träningsmodell* modulen tooone och hello utdata från hello *utvärdera modell* modulen toohello andra.</span><span class="sxs-lookup"><span data-stu-id="e641a-138">Connect hello output of hello *Train Model* module tooone and hello output of hello *Evaluate Model* module toohello other.</span></span> <span data-ttu-id="e641a-139">Hej web service utdata för **Träningsmodell** ger oss hello ny tränad modell.</span><span class="sxs-lookup"><span data-stu-id="e641a-139">hello web service output for **Train Model** gives us hello new trained model.</span></span> <span data-ttu-id="e641a-140">Hej utdata kopplade för**utvärdera modell** returnerar att modulen utdata, vilket är hello prestandaresultat.</span><span class="sxs-lookup"><span data-stu-id="e641a-140">hello output attached too**Evaluate Model** returns that module’s output, which is hello performance results.</span></span>
5. <span data-ttu-id="e641a-141">Klicka på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="e641a-141">Click **Run**.</span></span> 

<span data-ttu-id="e641a-142">Därefter måste du distribuera hello träningsexperiment som en webbtjänst som producerar en tränad modell och utvärderingsresultat av modellen.</span><span class="sxs-lookup"><span data-stu-id="e641a-142">Next you must deploy hello training experiment as a web service that produces a trained model and model evaluation results.</span></span> <span data-ttu-id="e641a-143">tooaccomplish är detta din nästa uppsättning åtgärder, beroende på om du arbetar med en klassisk webbtjänst eller en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="e641a-143">tooaccomplish this, your next set of actions are dependent on whether you are working with a Classic web service or a New web service.</span></span>  

<span data-ttu-id="e641a-144">**Klassiska webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="e641a-144">**Classic web service**</span></span>

<span data-ttu-id="e641a-145">Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **distribuera webbtjänsten [klassisk]**.</span><span class="sxs-lookup"><span data-stu-id="e641a-145">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="e641a-146">hello webbtjänsten **instrumentpanelen** visas med hello API-nyckel och hello API hjälpsidan för Batch-körningen.</span><span class="sxs-lookup"><span data-stu-id="e641a-146">hello Web Service **Dashboard** is displayed with hello API Key and hello API help page for Batch Execution.</span></span> <span data-ttu-id="e641a-147">Endast hello Batch Execution-metoden kan användas för att skapa tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="e641a-147">Only hello Batch Execution method can be used for creating Trained Models.</span></span>

<span data-ttu-id="e641a-148">**Ny webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="e641a-148">**New web service**</span></span>

<span data-ttu-id="e641a-149">Hello längst ned på hello experimentet klickar du på **konfigurera Web Service** och välj **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="e641a-149">At hello bottom of hello experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="e641a-150">hello Web Service Azure Machine Learning-webbtjänster portal öppnar toohello distribuera tjänsten webbsidan.</span><span class="sxs-lookup"><span data-stu-id="e641a-150">hello Web Service Azure Machine Learning Web Services portal opens toohello Deploy web service page.</span></span> <span data-ttu-id="e641a-151">Ange ett namn för webbtjänsten och välj en betalningsplan, och klicka sedan **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="e641a-151">Type a name for your web service and choose a payment plan, then click **Deploy**.</span></span> <span data-ttu-id="e641a-152">Endast hello Batch Execution-metoden kan användas för att skapa tränade modeller</span><span class="sxs-lookup"><span data-stu-id="e641a-152">Only hello Batch Execution method can be used for creating Trained Models</span></span>

<span data-ttu-id="e641a-153">I båda fallen när experimentet har slutförts körs hello resulterande arbetsflödet ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="e641a-153">In either case, after experiment has completed running, hello resulting workflow should look as follows:</span></span>

![Resulterande arbetsflödet när du kör.][4]



## <a name="retrain-hello-model-with-new-data-using-bes"></a><span data-ttu-id="e641a-155">Träna om hello modellen med nya data med BES</span><span class="sxs-lookup"><span data-stu-id="e641a-155">Retrain hello model with new data using BES</span></span>
<span data-ttu-id="e641a-156">I det här exemplet använder du C# toocreate hello omtränings program.</span><span class="sxs-lookup"><span data-stu-id="e641a-156">For this example, you are using C# toocreate hello retraining application.</span></span> <span data-ttu-id="e641a-157">Du kan också använda hello Python eller R exempel kod tooaccomplish den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="e641a-157">You can also use hello Python or R sample code tooaccomplish this task.</span></span>

<span data-ttu-id="e641a-158">toocall hello Omtränings-API: er:</span><span class="sxs-lookup"><span data-stu-id="e641a-158">toocall hello Retraining APIs:</span></span>

1. <span data-ttu-id="e641a-159">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="e641a-159">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="e641a-160">Logga in toohello Machine Learning-webbtjänst-portalen.</span><span class="sxs-lookup"><span data-stu-id="e641a-160">Sign in toohello Machine Learning Web Service portal.</span></span>
3. <span data-ttu-id="e641a-161">Om du arbetar med en klassisk webbtjänst klickar du på **klassiska webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="e641a-161">If you are working with a Classic web service, click **Classic Web Services**.</span></span>
   1. <span data-ttu-id="e641a-162">Klicka på hello-webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="e641a-162">Click hello web service you are working with.</span></span>
   2. <span data-ttu-id="e641a-163">Klicka på hello standardslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e641a-163">Click hello default endpoint.</span></span>
   3. <span data-ttu-id="e641a-164">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="e641a-164">Click **Consume**.</span></span>
   4. <span data-ttu-id="e641a-165">Längst ned hello hello **förbruka** i hello sidan **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="e641a-165">At hello bottom of hello **Consume** page, in hello **Sample Code** section, click **Batch**.</span></span>
   5. <span data-ttu-id="e641a-166">Fortsätt toostep 5 i den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="e641a-166">Continue toostep 5 of this procedure.</span></span>
4. <span data-ttu-id="e641a-167">Om du arbetar med en ny webbtjänst klickar du på **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="e641a-167">If you are working with a New web service, click **Web Services**.</span></span>
   1. <span data-ttu-id="e641a-168">Klicka på hello-webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="e641a-168">Click hello web service you are working with.</span></span>
   2. <span data-ttu-id="e641a-169">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="e641a-169">Click **Consume**.</span></span>
   3. <span data-ttu-id="e641a-170">Längst ned hello hello förbruka i sidan hello **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="e641a-170">At hello bottom of hello Consume page, in hello **Sample Code** section, click **Batch**.</span></span>
5. <span data-ttu-id="e641a-171">Kopiera hello C# exempelkod för batchkörning och klistra in den i Program.cs-filen för hello kontrollerar hello namnområde intakt.</span><span class="sxs-lookup"><span data-stu-id="e641a-171">Copy hello sample C# code for batch execution and paste it into hello Program.cs file, making sure hello namespace remains intact.</span></span>

<span data-ttu-id="e641a-172">Lägg till hello Nuget-paketet Microsoft.AspNet.WebApi.Client som anges i hello kommentarer.</span><span class="sxs-lookup"><span data-stu-id="e641a-172">Add hello Nuget package Microsoft.AspNet.WebApi.Client as specified in hello comments.</span></span> <span data-ttu-id="e641a-173">tooadd hello referens tooMicrosoft.WindowsAzure.Storage.dll kanske måste du först tooinstall hello-klientbibliotek för Microsoft Azure storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="e641a-173">tooadd hello reference tooMicrosoft.WindowsAzure.Storage.dll, you might first need tooinstall hello client library for Microsoft Azure storage services.</span></span> <span data-ttu-id="e641a-174">Mer information finns i [Windows lagringstjänster](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="e641a-174">For more information, see [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

### <a name="update-hello-apikey-declaration"></a><span data-ttu-id="e641a-175">Uppdatera hello apikey förklaring</span><span class="sxs-lookup"><span data-stu-id="e641a-175">Update hello apikey declaration</span></span>
<span data-ttu-id="e641a-176">Leta upp hello **apikey** deklaration.</span><span class="sxs-lookup"><span data-stu-id="e641a-176">Locate hello **apikey** declaration.</span></span>

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

<span data-ttu-id="e641a-177">I hello **grundläggande förbrukning info** avsnitt i hello **förbruka** , leta upp hello primärnyckel och kopiera den toohello **apikey** deklaration.</span><span class="sxs-lookup"><span data-stu-id="e641a-177">In hello **Basic consumption info** section of hello **Consume** page, locate hello primary key and copy it toohello **apikey** declaration.</span></span>

### <a name="update-hello-azure-storage-information"></a><span data-ttu-id="e641a-178">Uppdatera hello Azure Storage-informationen</span><span class="sxs-lookup"><span data-stu-id="e641a-178">Update hello Azure Storage information</span></span>
<span data-ttu-id="e641a-179">Överför en fil från en lokal enhet (till exempel ”C:\temp\CensusIpnput.csv”) tooAzure lagring hello BES exempelkod, bearbetar den och skriver hello resultaten tillbaka tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="e641a-179">hello BES sample code uploads a file from a local drive (For example "C:\temp\CensusIpnput.csv") tooAzure Storage, processes it, and writes hello results back tooAzure Storage.</span></span>  

<span data-ttu-id="e641a-180">tooaccomplish den här uppgiften, måste du hämta hello namn och nyckel behållaren information om Lagringskonto för ditt lagringskonto hello klassiska Azure-portalen och hello uppdatera motsvarande värden i hello kod.</span><span class="sxs-lookup"><span data-stu-id="e641a-180">tooaccomplish this task, you must retrieve hello Storage account name, key, and container information for your Storage account from hello classic Azure portal and hello update corresponding values in hello code.</span></span> 

1. <span data-ttu-id="e641a-181">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e641a-181">Sign in toohello classic Azure portal.</span></span>
2. <span data-ttu-id="e641a-182">Klicka på hello navigeringen till vänster i kolumnen **lagring**.</span><span class="sxs-lookup"><span data-stu-id="e641a-182">In hello left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="e641a-183">Välj en toostore hello retrained hello listan över storage-konton och modell.</span><span class="sxs-lookup"><span data-stu-id="e641a-183">From hello list of storage accounts, select one toostore hello retrained model.</span></span>
4. <span data-ttu-id="e641a-184">Hello längst hello-sidan, klickar du på **hantera åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="e641a-184">At hello bottom of hello page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="e641a-185">Kopiera och spara hello **primära åtkomstnyckeln** och Stäng hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e641a-185">Copy and save hello **Primary Access Key** and close hello dialog.</span></span> 
6. <span data-ttu-id="e641a-186">Hello överst på hello-sidan, klickar du på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="e641a-186">At hello top of hello page, click **Containers**.</span></span>
7. <span data-ttu-id="e641a-187">Välj en befintlig behållare eller skapa en ny och spara hello namn.</span><span class="sxs-lookup"><span data-stu-id="e641a-187">Select an existing container or create a new one and save hello name.</span></span>

<span data-ttu-id="e641a-188">Leta upp hello *StorageAccountName*, *StorageAccountKey*, och *StorageContainerName* deklarationer och uppdatera hello värden som du sparade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e641a-188">Locate hello *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations and update hello values you saved from hello Azure portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

<span data-ttu-id="e641a-189">Du måste också kontrollera hello indatafilen är tillgänglig på hello-plats som du anger i hello kod.</span><span class="sxs-lookup"><span data-stu-id="e641a-189">You also must ensure hello input file is available at hello location you specify in hello code.</span></span> 

### <a name="specify-hello-output-location"></a><span data-ttu-id="e641a-190">Ange hello platsen</span><span class="sxs-lookup"><span data-stu-id="e641a-190">Specify hello output location</span></span>
<span data-ttu-id="e641a-191">När du anger hello platsen i hello begära nyttolast hello-tillägget för hello-fil som anges i *RelativeLocation* måste anges som ilearner.</span><span class="sxs-lookup"><span data-stu-id="e641a-191">When specifying hello output location in hello Request Payload, hello extension of hello file specified in *RelativeLocation* must be specified as ilearner.</span></span> 

<span data-ttu-id="e641a-192">Se följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="e641a-192">See hello following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you would like toouse for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> <span data-ttu-id="e641a-193">hello namnen på utdata-platser kan skilja sig från hello i den här genomgången baserat på hello ordning i vilken du har lagt till hello web service utdata moduler.</span><span class="sxs-lookup"><span data-stu-id="e641a-193">hello names of your output locations may be different from hello ones in this walkthrough based on hello order in which you added hello web service output modules.</span></span> <span data-ttu-id="e641a-194">Eftersom du ställer in den här träningsexperiment med två utdata inkludera information om lagring för båda hello resultat.</span><span class="sxs-lookup"><span data-stu-id="e641a-194">Since you set up this training experiment with two outputs, hello results include storage location information for both of them.</span></span>  
> 
> 

![Omtränings utdata][6]

<span data-ttu-id="e641a-196">Diagram över 4: Omtränings utdata.</span><span class="sxs-lookup"><span data-stu-id="e641a-196">Diagram 4: Retraining output.</span></span>

## <a name="evaluate-hello-retraining-results"></a><span data-ttu-id="e641a-197">Utvärdera hello Omtränings resultat</span><span class="sxs-lookup"><span data-stu-id="e641a-197">Evaluate hello Retraining Results</span></span>
<span data-ttu-id="e641a-198">När du kör programmet hello hello utdata innehåller hello URL och SAS-token nödvändiga tooaccess hello utvärderingsresultaten.</span><span class="sxs-lookup"><span data-stu-id="e641a-198">When you run hello application, hello output includes hello URL and SAS token necessary tooaccess hello evaluation results.</span></span>

<span data-ttu-id="e641a-199">Du kan se hello prestandaresultat hello retrained modellen genom att kombinera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* hello utdata resultaten för *output2* (som visas i föregående omtränings utdata avbildningen hello) och klistra in hello fullständiga URL: en i hello webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="e641a-199">You can see hello performance results of hello retrained model by combining hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results for *output2* (as shown in hello preceding retraining output image) and pasting hello complete URL in hello browser address bar.</span></span>  

<span data-ttu-id="e641a-200">Undersöka hello resultat toodetermine om hello nyligen utbildade modellen presterar bra tillräckligt med tooreplace hello befintliga en.</span><span class="sxs-lookup"><span data-stu-id="e641a-200">Examine hello results toodetermine whether hello newly trained model performs well enough tooreplace hello existing one.</span></span>

<span data-ttu-id="e641a-201">Kopiera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* från hello utdataresultat som ska användas för dem under hello omtränings process.</span><span class="sxs-lookup"><span data-stu-id="e641a-201">Copy hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results, you will use them during hello retraining process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e641a-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e641a-202">Next steps</span></span>
<span data-ttu-id="e641a-203">Om du har distribuerat hello förutsägande webbtjänsten genom att klicka på **distribuera webbtjänsten [klassisk]**, se [träna om en klassisk webbtjänst](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-203">If you deployed hello predictive web service by clicking **Deploy Web Service [Classic]**, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="e641a-204">Om du har distribuerat hello förutsägande webbtjänsten genom att klicka på **distribuera webbtjänsten [ny]**, se [träna om en ny webbtjänst med Machine Learning Management-cmdlets för hello](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="e641a-204">If you deployed hello predictive web service by clicking **Deploy Web Service [New]**, see [Retrain a New web service using hello Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<!-- Retrain a New web service using hello Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
