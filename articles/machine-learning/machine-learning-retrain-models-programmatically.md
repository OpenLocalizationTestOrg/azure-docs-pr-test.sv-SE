---
title: "Träna om Machine Learning-modeller via programmering | Microsoft Docs"
description: "Lär dig mer om att träna om en modell och uppdatera webbtjänsten för att använda den nya tränade modellen i Azure Machine Learning programmässigt."
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
ms.openlocfilehash: cf7a39e14a935d0d0e0df07e66a8f37480ec9687
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="retrain-machine-learning-models-programmatically"></a><span data-ttu-id="07bdf-103">Omtrimning av Azure Machine Learning-modeller via programmering</span><span class="sxs-lookup"><span data-stu-id="07bdf-103">Retrain Machine Learning models programmatically</span></span>
<span data-ttu-id="07bdf-104">I den här genomgången får lära du dig att träna programmässigt om en Azure Machine Learning-webbtjänst med hjälp av C# och Machine Learning Batch Execution service.</span><span class="sxs-lookup"><span data-stu-id="07bdf-104">In this walkthrough, you will learn how to programmatically retrain an Azure Machine Learning Web Service using C# and the Machine Learning Batch Execution service.</span></span>

<span data-ttu-id="07bdf-105">När du har retrained modellen, visar hur du uppdaterar modellen i förutsägande webbtjänsten följande genomgång:</span><span class="sxs-lookup"><span data-stu-id="07bdf-105">Once you have retrained the model, the following walkthroughs show how to update the model in your predictive web service:</span></span>

* <span data-ttu-id="07bdf-106">Om du har distribuerat en klassisk webbtjänst i Machine Learning-webbtjänster portal finns [träna om en klassisk webbtjänst](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-106">If you deployed a Classic web service in the Machine Learning Web Services portal, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span> 
* <span data-ttu-id="07bdf-107">Om du har distribuerat en ny webbtjänst finns [träna om en ny webbtjänst med hjälp av Machine Learning Management-cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-107">If you deployed a New web service, see [Retrain a New web service using the Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<span data-ttu-id="07bdf-108">En översikt över omtränings-processen, se [träna om en Maskininlärningsmodell](machine-learning-retrain-machine-learning-model.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-108">For an overview of the retraining process, see [Retrain a Machine Learning Model](machine-learning-retrain-machine-learning-model.md).</span></span>

<span data-ttu-id="07bdf-109">Om du vill börja med din befintliga nya Azure Resource Manager baserad webbtjänst finns [träna om en befintlig förutsägande webbtjänst](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-109">If you want to start with your existing New Azure Resource Manager based web service, see [Retrain an existing Predictive web service](machine-learning-retrain-existing-resource-manager-based-web-service.md).</span></span>

## <a name="create-a-training-experiment"></a><span data-ttu-id="07bdf-110">Skapa ett experiment utbildning</span><span class="sxs-lookup"><span data-stu-id="07bdf-110">Create a training experiment</span></span>
<span data-ttu-id="07bdf-111">För det här exemplet använder du ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset” från Microsoft Azure Machine Learning-prover.</span><span class="sxs-lookup"><span data-stu-id="07bdf-111">For this example, you will use "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset" from the Microsoft Azure Machine Learning samples.</span></span> 

<span data-ttu-id="07bdf-112">Skapa experimentet:</span><span class="sxs-lookup"><span data-stu-id="07bdf-112">To create the experiment:</span></span>

1. <span data-ttu-id="07bdf-113">Logga in till Microsoft Azure Maskininlärning Studio.</span><span class="sxs-lookup"><span data-stu-id="07bdf-113">Sign into to Microsoft Azure Machine Learning Studio.</span></span> 
2. <span data-ttu-id="07bdf-114">Klicka på nedre högra hörnet av instrumentpanelen, **ny**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-114">On the bottom right corner of the dashboard, click **New**.</span></span>
3. <span data-ttu-id="07bdf-115">Välj exempel 5 från Microsoft-Samples.</span><span class="sxs-lookup"><span data-stu-id="07bdf-115">From the Microsoft Samples, select Sample 5.</span></span>
4. <span data-ttu-id="07bdf-116">Byt namn på experimentet överst på arbetsytan för experimentet väljer du namnet på experiment ”exempel 5: tåg och Test, utvärdera för binär klassificering: vuxna Dataset”.</span><span class="sxs-lookup"><span data-stu-id="07bdf-116">To rename the experiment, at the top of the experiment canvas, select the experiment name "Sample 5: Train, Test, Evaluate for Binary Classification: Adult Dataset".</span></span>
5. <span data-ttu-id="07bdf-117">Typ av inventering modell.</span><span class="sxs-lookup"><span data-stu-id="07bdf-117">Type Census Model.</span></span>
6. <span data-ttu-id="07bdf-118">Längst ned i arbetsytan för experimentet klickar du på **kör**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-118">At the bottom of the experiment canvas, click **Run**.</span></span>
7. <span data-ttu-id="07bdf-119">Klicka på **Konfigurera webbtjänsten** och välj **Omtränings webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-119">Click **Set Up web service** and select **Retraining web service**.</span></span> 

<span data-ttu-id="07bdf-120">Nedan visas första försöket.</span><span class="sxs-lookup"><span data-stu-id="07bdf-120">The following shows the initial experiment.</span></span>
   
   ![Första försöket.][2]


## <a name="create-a-predictive-experiment-and-publish-as-a-web-service"></a><span data-ttu-id="07bdf-122">Skapa en prediktivt experiment och publicera som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="07bdf-122">Create a predictive experiment and publish as a web service</span></span>
<span data-ttu-id="07bdf-123">Nu kan du skapa ett Predicative Experiment.</span><span class="sxs-lookup"><span data-stu-id="07bdf-123">Next you create a Predicative Experiment.</span></span>

1. <span data-ttu-id="07bdf-124">Längst ned i arbetsytan för experimentet klickar du på **konfigurera Web Service** och välj **förutsägande webbtjänsten**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-124">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Predictive Web Service**.</span></span> <span data-ttu-id="07bdf-125">Detta sparar modellen som en Tränad modell och lägger till web service ingående och utgående moduler.</span><span class="sxs-lookup"><span data-stu-id="07bdf-125">This saves the model as a Trained Model and adds web service Input and Output modules.</span></span> 
2. <span data-ttu-id="07bdf-126">Klicka på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="07bdf-126">Click **Run**.</span></span> 
3. <span data-ttu-id="07bdf-127">När experimentet har slutförts klickar du på **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-127">After the experiment has finished running, click **Deploy Web Service [Classic]** or **Deploy Web Service [New]**.</span></span>

> [!NOTE] 
> <span data-ttu-id="07bdf-128">Om du vill distribuera en ny webbtjänst måste du ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="07bdf-128">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="07bdf-129">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-129">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

## <a name="deploy-the-training-experiment-as-a-training-web-service"></a><span data-ttu-id="07bdf-130">Distribuera utbildning experiment som en webbtjänst för utbildning</span><span class="sxs-lookup"><span data-stu-id="07bdf-130">Deploy the training experiment as a Training web service</span></span>
<span data-ttu-id="07bdf-131">För att träna om den tränade modellen, måste du distribuera träningsexperiment som du har skapat som en webbtjänst för Retraining.</span><span class="sxs-lookup"><span data-stu-id="07bdf-131">To retrain the trained model, you must deploy the training experiment that you created as a Retraining web service.</span></span> <span data-ttu-id="07bdf-132">Den här webbtjänsten måste en *Web Service utdata* modul som är ansluten till den  *[Träningsmodell] [ train-model]*  modulen för att kunna skapa nya tränats modeller.</span><span class="sxs-lookup"><span data-stu-id="07bdf-132">This web service needs a *Web Service Output* module connected to the *[Train Model][train-model]* module, to be able to produce new trained models.</span></span>

1. <span data-ttu-id="07bdf-133">För att återgå till utbildning experimentet klickar du på ikonen experiment i den vänstra rutan och klicka på experiment med namnet inventering modellen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-133">To return to the training experiment, click the Experiments icon in the left pane, then click the experiment named Census Model.</span></span>  
2. <span data-ttu-id="07bdf-134">Skriv i sökrutan Experiment sökobjekt webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="07bdf-134">In the Search Experiment Items search box, type web service.</span></span> 
3. <span data-ttu-id="07bdf-135">Dra en *webbtjänst* till arbetsytan för experimentet och koppla dess utdata till den *Rensa Data som saknas* modul.</span><span class="sxs-lookup"><span data-stu-id="07bdf-135">Drag a *Web Service Input* module onto the experiment canvas and connect its output to the *Clean Missing Data* module.</span></span>  <span data-ttu-id="07bdf-136">Detta säkerställer att omtränings data bearbetas på samma sätt som den ursprungliga informationen för utbildning.</span><span class="sxs-lookup"><span data-stu-id="07bdf-136">This ensures that your retraining data is processed the same way as your original training data.</span></span>
4. <span data-ttu-id="07bdf-137">Dra två *webbtjänsten utdata* moduler till arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="07bdf-137">Drag two *web service Output* modules onto the experiment canvas.</span></span> <span data-ttu-id="07bdf-138">Ansluta utdata från den *Träningsmodell* modul till en och utdata från den *utvärdera modell* modul till den andra.</span><span class="sxs-lookup"><span data-stu-id="07bdf-138">Connect the output of the *Train Model* module to one and the output of the *Evaluate Model* module to the other.</span></span> <span data-ttu-id="07bdf-139">Web service-utdata för **Träningsmodell** ger oss den nya tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-139">The web service output for **Train Model** gives us the new trained model.</span></span> <span data-ttu-id="07bdf-140">Utdata som är kopplade till **utvärdera modell** returnerar att modulen utdata, vilket är prestandaresultat.</span><span class="sxs-lookup"><span data-stu-id="07bdf-140">The output attached to **Evaluate Model** returns that module’s output, which is the performance results.</span></span>
5. <span data-ttu-id="07bdf-141">Klicka på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="07bdf-141">Click **Run**.</span></span> 

<span data-ttu-id="07bdf-142">Därefter måste du distribuera utbildning experiment som en webbtjänst som producerar en tränad modell och utvärderingsresultat av modellen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-142">Next you must deploy the training experiment as a web service that produces a trained model and model evaluation results.</span></span> <span data-ttu-id="07bdf-143">För att åstadkomma detta kan din nästa uppsättning åtgärder, beroende på om du arbetar med en klassisk webbtjänst eller en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="07bdf-143">To accomplish this, your next set of actions are dependent on whether you are working with a Classic web service or a New web service.</span></span>  

<span data-ttu-id="07bdf-144">**Klassiska webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="07bdf-144">**Classic web service**</span></span>

<span data-ttu-id="07bdf-145">Längst ned i arbetsytan för experimentet klickar du på **konfigurera Web Service** och välj **distribuera webbtjänsten [klassisk]**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-145">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [Classic]**.</span></span> <span data-ttu-id="07bdf-146">Webbtjänsten **instrumentpanelen** visas med API-nyckel och API-hjälpsidan för Batch-körningen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-146">The Web Service **Dashboard** is displayed with the API Key and the API help page for Batch Execution.</span></span> <span data-ttu-id="07bdf-147">Batch Execution metoden kan användas för att skapa tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="07bdf-147">Only the Batch Execution method can be used for creating Trained Models.</span></span>

<span data-ttu-id="07bdf-148">**Ny webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="07bdf-148">**New web service**</span></span>

<span data-ttu-id="07bdf-149">Längst ned i arbetsytan för experimentet klickar du på **konfigurera Web Service** och välj **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-149">At the bottom of the experiment canvas, click **Set Up Web Service** and select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="07bdf-150">Web Service Azure Machine Learning-webbtjänster portal öppnar sidan distribuera tjänsten.</span><span class="sxs-lookup"><span data-stu-id="07bdf-150">The Web Service Azure Machine Learning Web Services portal opens to the Deploy web service page.</span></span> <span data-ttu-id="07bdf-151">Ange ett namn för webbtjänsten och välj en betalningsplan, och klicka sedan **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-151">Type a name for your web service and choose a payment plan, then click **Deploy**.</span></span> <span data-ttu-id="07bdf-152">Endast Batch Execution-metoden kan användas för att skapa tränade modeller</span><span class="sxs-lookup"><span data-stu-id="07bdf-152">Only the Batch Execution method can be used for creating Trained Models</span></span>

<span data-ttu-id="07bdf-153">I båda fallen när experimentet har slutförts, resulterande arbetsflödet ska se ut så här:</span><span class="sxs-lookup"><span data-stu-id="07bdf-153">In either case, after experiment has completed running, the resulting workflow should look as follows:</span></span>

![Resulterande arbetsflödet när du kör.][4]



## <a name="retrain-the-model-with-new-data-using-bes"></a><span data-ttu-id="07bdf-155">Träna om modellen med nya data med BES</span><span class="sxs-lookup"><span data-stu-id="07bdf-155">Retrain the model with new data using BES</span></span>
<span data-ttu-id="07bdf-156">I det här exemplet använder du C# skapa omtränings programmet.</span><span class="sxs-lookup"><span data-stu-id="07bdf-156">For this example, you are using C# to create the retraining application.</span></span> <span data-ttu-id="07bdf-157">Du kan också använda exempelkoden Python eller R för att utföra den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="07bdf-157">You can also use the Python or R sample code to accomplish this task.</span></span>

<span data-ttu-id="07bdf-158">Att anropa Omtränings-API:</span><span class="sxs-lookup"><span data-stu-id="07bdf-158">To call the Retraining APIs:</span></span>

1. <span data-ttu-id="07bdf-159">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-159">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="07bdf-160">Logga in på portalen Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="07bdf-160">Sign in to the Machine Learning Web Service portal.</span></span>
3. <span data-ttu-id="07bdf-161">Om du arbetar med en klassisk webbtjänst klickar du på **klassiska webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-161">If you are working with a Classic web service, click **Classic Web Services**.</span></span>
   1. <span data-ttu-id="07bdf-162">Klicka på den webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="07bdf-162">Click the web service you are working with.</span></span>
   2. <span data-ttu-id="07bdf-163">Klicka på standardslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="07bdf-163">Click the default endpoint.</span></span>
   3. <span data-ttu-id="07bdf-164">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-164">Click **Consume**.</span></span>
   4. <span data-ttu-id="07bdf-165">Längst ned i den **förbruka** sidan den **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-165">At the bottom of the **Consume** page, in the **Sample Code** section, click **Batch**.</span></span>
   5. <span data-ttu-id="07bdf-166">Fortsätt till steg 5 i den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="07bdf-166">Continue to step 5 of this procedure.</span></span>
4. <span data-ttu-id="07bdf-167">Om du arbetar med en ny webbtjänst klickar du på **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-167">If you are working with a New web service, click **Web Services**.</span></span>
   1. <span data-ttu-id="07bdf-168">Klicka på den webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="07bdf-168">Click the web service you are working with.</span></span>
   2. <span data-ttu-id="07bdf-169">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-169">Click **Consume**.</span></span>
   3. <span data-ttu-id="07bdf-170">AT längst ned i förbruka sidan den **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-170">At the bottom of the Consume page, in the **Sample Code** section, click **Batch**.</span></span>
5. <span data-ttu-id="07bdf-171">Kopiera C# exempelkod för batchkörning och klistra in den i filen Program.cs, vilket gör att namnområdet intakt.</span><span class="sxs-lookup"><span data-stu-id="07bdf-171">Copy the sample C# code for batch execution and paste it into the Program.cs file, making sure the namespace remains intact.</span></span>

<span data-ttu-id="07bdf-172">Lägg till Nuget-paketet Microsoft.AspNet.WebApi.Client som anges i kommentarerna.</span><span class="sxs-lookup"><span data-stu-id="07bdf-172">Add the Nuget package Microsoft.AspNet.WebApi.Client as specified in the comments.</span></span> <span data-ttu-id="07bdf-173">Om du vill lägga till en referens till Microsoft.WindowsAzure.Storage.dll, kan du först behöva installera klientbiblioteket för Microsoft Azure storage-tjänster.</span><span class="sxs-lookup"><span data-stu-id="07bdf-173">To add the reference to Microsoft.WindowsAzure.Storage.dll, you might first need to install the client library for Microsoft Azure storage services.</span></span> <span data-ttu-id="07bdf-174">Mer information finns i [Windows lagringstjänster](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="07bdf-174">For more information, see [Windows Storage Services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

### <a name="update-the-apikey-declaration"></a><span data-ttu-id="07bdf-175">Uppdatera apikey deklarationen</span><span class="sxs-lookup"><span data-stu-id="07bdf-175">Update the apikey declaration</span></span>
<span data-ttu-id="07bdf-176">Leta upp den **apikey** deklaration.</span><span class="sxs-lookup"><span data-stu-id="07bdf-176">Locate the **apikey** declaration.</span></span>

    const string apiKey = "abc123"; // Replace this with the API key for the web service

<span data-ttu-id="07bdf-177">I den **grundläggande förbrukning info** avsnitt i den **förbruka** , leta upp den primära nyckeln och kopiera den till den **apikey** deklaration.</span><span class="sxs-lookup"><span data-stu-id="07bdf-177">In the **Basic consumption info** section of the **Consume** page, locate the primary key and copy it to the **apikey** declaration.</span></span>

### <a name="update-the-azure-storage-information"></a><span data-ttu-id="07bdf-178">Uppdatera Azure Storage-informationen</span><span class="sxs-lookup"><span data-stu-id="07bdf-178">Update the Azure Storage information</span></span>
<span data-ttu-id="07bdf-179">Exempelkoden BES överför en fil från en lokal enhet (till exempel ”C:\temp\CensusIpnput.csv”) till Azure Storage, bearbetar den och skriver resultatet till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="07bdf-179">The BES sample code uploads a file from a local drive (For example "C:\temp\CensusIpnput.csv") to Azure Storage, processes it, and writes the results back to Azure Storage.</span></span>  

<span data-ttu-id="07bdf-180">För att åstadkomma detta måste du hämta lagring namn och nyckel behållaren kontoinformationen för ditt lagringskonto från den klassiska Azure-portalen och uppdatera motsvarande värden i koden.</span><span class="sxs-lookup"><span data-stu-id="07bdf-180">To accomplish this task, you must retrieve the Storage account name, key, and container information for your Storage account from the classic Azure portal and the update corresponding values in the code.</span></span> 

1. <span data-ttu-id="07bdf-181">Logga in på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-181">Sign in to the classic Azure portal.</span></span>
2. <span data-ttu-id="07bdf-182">Klicka på den vänstra navigeringsfönstret i kolumnen **lagring**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-182">In the left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="07bdf-183">Välj en för att lagra retrained modellen från listan över storage-konton.</span><span class="sxs-lookup"><span data-stu-id="07bdf-183">From the list of storage accounts, select one to store the retrained model.</span></span>
4. <span data-ttu-id="07bdf-184">Längst ned på sidan klickar du på **hantera åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-184">At the bottom of the page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="07bdf-185">Kopiera och spara den **primära åtkomstnyckeln** och Stäng dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="07bdf-185">Copy and save the **Primary Access Key** and close the dialog.</span></span> 
6. <span data-ttu-id="07bdf-186">Överst på sidan, klickar du på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="07bdf-186">At the top of the page, click **Containers**.</span></span>
7. <span data-ttu-id="07bdf-187">Välj en befintlig behållare eller skapa en ny och spara namnet.</span><span class="sxs-lookup"><span data-stu-id="07bdf-187">Select an existing container or create a new one and save the name.</span></span>

<span data-ttu-id="07bdf-188">Leta upp den *StorageAccountName*, *StorageAccountKey*, och *StorageContainerName* deklarationer och uppdatera värden som du sparade från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-188">Locate the *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations and update the values you saved from the Azure portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure Storage Account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage Key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage Container name

<span data-ttu-id="07bdf-189">Du måste också kontrollera indatafilen är tillgänglig på den plats du anger i koden.</span><span class="sxs-lookup"><span data-stu-id="07bdf-189">You also must ensure the input file is available at the location you specify in the code.</span></span> 

### <a name="specify-the-output-location"></a><span data-ttu-id="07bdf-190">Ange platsen för utdata</span><span class="sxs-lookup"><span data-stu-id="07bdf-190">Specify the output location</span></span>
<span data-ttu-id="07bdf-191">När du anger platsen i nyttolasten för begäran om tillägg av filen anges i *RelativeLocation* måste anges som ilearner.</span><span class="sxs-lookup"><span data-stu-id="07bdf-191">When specifying the output location in the Request Payload, the extension of the file specified in *RelativeLocation* must be specified as ilearner.</span></span> 

<span data-ttu-id="07bdf-192">Se följande exempel:</span><span class="sxs-lookup"><span data-stu-id="07bdf-192">See the following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you would like to use for your output file, and valid file extension (usually .csv for scoring results, or .ilearner for trained models)*/
            }
        },

> [!NOTE]
> <span data-ttu-id="07bdf-193">Namnen på utdata-platser kan skilja sig från de som finns i den här genomgången utifrån den ordning som du har lagt till web service utdata moduler.</span><span class="sxs-lookup"><span data-stu-id="07bdf-193">The names of your output locations may be different from the ones in this walkthrough based on the order in which you added the web service output modules.</span></span> <span data-ttu-id="07bdf-194">Eftersom du ställt in den här träningsexperiment med två utdata, inkluderar resultaten platsinformation för lagring för båda.</span><span class="sxs-lookup"><span data-stu-id="07bdf-194">Since you set up this training experiment with two outputs, the results include storage location information for both of them.</span></span>  
> 
> 

![Omtränings utdata][6]

<span data-ttu-id="07bdf-196">Diagram över 4: Omtränings utdata.</span><span class="sxs-lookup"><span data-stu-id="07bdf-196">Diagram 4: Retraining output.</span></span>

## <a name="evaluate-the-retraining-results"></a><span data-ttu-id="07bdf-197">Utvärdera Omtränings resultaten</span><span class="sxs-lookup"><span data-stu-id="07bdf-197">Evaluate the Retraining Results</span></span>
<span data-ttu-id="07bdf-198">När du kör programmet innehåller URL: en och SAS-token krävs för att komma åt utvärderingsresultaten utdata.</span><span class="sxs-lookup"><span data-stu-id="07bdf-198">When you run the application, the output includes the URL and SAS token necessary to access the evaluation results.</span></span>

<span data-ttu-id="07bdf-199">Du kan se prestandaresultat retrained modellen genom att kombinera den *BaseLocation*, *RelativeLocation*, och *SasBlobToken* från resultatet för *output2* (som visas i föregående omtränings utdata avbildningen) och klistra in den fullständiga URL: en i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="07bdf-199">You can see the performance results of the retrained model by combining the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results for *output2* (as shown in the preceding retraining output image) and pasting the complete URL in the browser address bar.</span></span>  

<span data-ttu-id="07bdf-200">Granska resultaten för att avgöra om den nyligen tränade modellen utför bra för att ersätta den befintliga versionen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-200">Examine the results to determine whether the newly trained model performs well enough to replace the existing one.</span></span>

<span data-ttu-id="07bdf-201">Kopiera den *BaseLocation*, *RelativeLocation*, och *SasBlobToken* utdata-resultaten ska du använda dem under omtränings-processen.</span><span class="sxs-lookup"><span data-stu-id="07bdf-201">Copy the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results, you will use them during the retraining process.</span></span>

## <a name="next-steps"></a><span data-ttu-id="07bdf-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="07bdf-202">Next steps</span></span>
<span data-ttu-id="07bdf-203">Om du har distribuerat förutsägande webbtjänsten genom att klicka på **distribuera webbtjänsten [klassisk]**, se [träna om en klassisk webbtjänst](machine-learning-retrain-a-classic-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-203">If you deployed the predictive web service by clicking **Deploy Web Service [Classic]**, see [Retrain a Classic web service](machine-learning-retrain-a-classic-web-service.md).</span></span>

<span data-ttu-id="07bdf-204">Om du har distribuerat förutsägande webbtjänsten genom att klicka på **distribuera webbtjänsten [ny]**, se [träna om en ny webbtjänst med hjälp av Machine Learning Management-cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="07bdf-204">If you deployed the predictive web service by clicking **Deploy Web Service [New]**, see [Retrain a New web service using the Machine Learning Management cmdlets](machine-learning-retrain-new-web-service-using-powershell.md).</span></span>

<!-- Retrain a New web service using the Machine Learning Management REST API -->


[1]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE01.png
[2]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE02.png
[3]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE03.png
[4]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE04.png
[5]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE05.png
[6]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE06.png
[7]: ./media/machine-learning-retrain-models-programmatically/machine-learning-retrain-models-programmatically-IMAGE07.png


<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
