---
title: "Träna om en befintlig förutsägande webbtjänst | Microsoft Docs"
description: "Lär dig mer om att träna om en modell och uppdatera webbtjänsten för att använda den nya tränade modellen i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
ms.openlocfilehash: bdc994daf441d397157f8e6cbcf84d72584927f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a><span data-ttu-id="20f52-103">Träna om en befintlig förutsägande webbtjänst</span><span class="sxs-lookup"><span data-stu-id="20f52-103">Retrain an existing predictive web service</span></span>
<span data-ttu-id="20f52-104">Det här dokumentet beskriver omtränings för följande scenario:</span><span class="sxs-lookup"><span data-stu-id="20f52-104">This document describes the retraining process for the following scenario:</span></span>

* <span data-ttu-id="20f52-105">Du har en träningsexperiment och en prediktivt experiment som du har distribuerat som en operationalized webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="20f52-105">You have a training experiment and a predictive experiment that you have deployed as an operationalized web service.</span></span>
* <span data-ttu-id="20f52-106">Du har nya data som du vill förutsägande webbtjänsten att utföra den bedömningen.</span><span class="sxs-lookup"><span data-stu-id="20f52-106">You have new data that you want your predictive web service to use to perform its scoring.</span></span>

> [!NOTE] 
> <span data-ttu-id="20f52-107">Om du vill distribuera en ny webbtjänst måste du ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="20f52-107">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="20f52-108">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="20f52-108">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="20f52-109">Från och med din befintliga webbtjänsten och experiment, måste du följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="20f52-109">Starting with your existing web service and experiments, you need to follow these steps:</span></span>

1. <span data-ttu-id="20f52-110">Uppdatera modellen.</span><span class="sxs-lookup"><span data-stu-id="20f52-110">Update the model.</span></span>
   1. <span data-ttu-id="20f52-111">Ändra experimentet utbildning för tjänsten indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="20f52-111">Modify your training experiment to allow for web service inputs and outputs.</span></span>
   2. <span data-ttu-id="20f52-112">Distribuera utbildning experiment som en omtränings-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="20f52-112">Deploy the training experiment as a retraining web service.</span></span>
   3. <span data-ttu-id="20f52-113">Använd den träningsexperiment Batch Execution Service BES-för att träna om modellen.</span><span class="sxs-lookup"><span data-stu-id="20f52-113">Use the training experiment's Batch Execution Service (BES) to retrain the model.</span></span>
2. <span data-ttu-id="20f52-114">Använda Azure Machine Learning PowerShell-cmdlets för att uppdatera prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="20f52-114">Use the Azure Machine Learning PowerShell cmdlets to update the predictive experiment.</span></span>
   1. <span data-ttu-id="20f52-115">Logga in på ditt konto i Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="20f52-115">Sign in to your Azure Resource Manager account.</span></span>
   2. <span data-ttu-id="20f52-116">Hämta web service definition.</span><span class="sxs-lookup"><span data-stu-id="20f52-116">Get the web service definition.</span></span>
   3. <span data-ttu-id="20f52-117">Exportera web service definition som JSON.</span><span class="sxs-lookup"><span data-stu-id="20f52-117">Export the web service definition as JSON.</span></span>
   4. <span data-ttu-id="20f52-118">Uppdatera referens till ilearner-blob i JSON.</span><span class="sxs-lookup"><span data-stu-id="20f52-118">Update the reference to the ilearner blob in the JSON.</span></span>
   5. <span data-ttu-id="20f52-119">Importera JSON till en web service definition.</span><span class="sxs-lookup"><span data-stu-id="20f52-119">Import the JSON into a web service definition.</span></span>
   6. <span data-ttu-id="20f52-120">Uppdatera webbtjänsten med en ny web service definition.</span><span class="sxs-lookup"><span data-stu-id="20f52-120">Update the web service with a new web service definition.</span></span>

## <a name="deploy-the-training-experiment"></a><span data-ttu-id="20f52-121">Distribuera utbildning experimentet</span><span class="sxs-lookup"><span data-stu-id="20f52-121">Deploy the training experiment</span></span>
<span data-ttu-id="20f52-122">Om du vill distribuera träningsexperiment som en omtränings webbtjänst, måste du lägga till web service in- och utdataenheter modellen.</span><span class="sxs-lookup"><span data-stu-id="20f52-122">To deploy the training experiment as a retraining web service, you must add web service inputs and outputs to the model.</span></span> <span data-ttu-id="20f52-123">Genom att ansluta en *Web Service utdata* modulen till arbetsytan för experimentet  *[Träningsmodell] [ train-model]*  modulen, aktivera utbildning experimentet Skapa en ny tränad modell som du kan använda i experimentet förutsägbara.</span><span class="sxs-lookup"><span data-stu-id="20f52-123">By connecting a *Web Service Output* module to the experiment's *[Train Model][train-model]* module, you enable the training experiment to produce a new trained model that you can use in your predictive experiment.</span></span> <span data-ttu-id="20f52-124">Om du har en *utvärdera modell* modulen, du kan även bifoga web service-utdata för att få utvärderingsresultaten som utdata.</span><span class="sxs-lookup"><span data-stu-id="20f52-124">If you have an *Evaluate Model* module, you can also attach web service output to get the evaluation results as output.</span></span>

<span data-ttu-id="20f52-125">Så här uppdaterar experimentet utbildning:</span><span class="sxs-lookup"><span data-stu-id="20f52-125">To update your training experiment:</span></span>

1. <span data-ttu-id="20f52-126">Ansluta en *Web Service inkommande* modulen till data-indata (till exempel en *Rensa Data som saknas* module).</span><span class="sxs-lookup"><span data-stu-id="20f52-126">Connect a *Web Service Input* module to your data input (for example, a *Clean Missing Data* module).</span></span> <span data-ttu-id="20f52-127">Vanligtvis vill du se till att dina indata behandlas på samma sätt som den ursprungliga informationen för utbildning.</span><span class="sxs-lookup"><span data-stu-id="20f52-127">Typically, you want to ensure that your input data is processed in the same way as your original training data.</span></span>
2. <span data-ttu-id="20f52-128">Ansluta en *Web Service utdata* modulen till utdataporten för din *Träningsmodell* modul.</span><span class="sxs-lookup"><span data-stu-id="20f52-128">Connect a *Web Service Output* module to the output of your *Train Model* module.</span></span>
3. <span data-ttu-id="20f52-129">Om du har en *utvärdera modell* modul och du vill att utdata utvärderingsresultaten, ansluta en *Web Service utdata* modulen till utdataporten för din *utvärdera modell* modul.</span><span class="sxs-lookup"><span data-stu-id="20f52-129">If you have an *Evaluate Model* module and you want to output the evaluation results, connect a *Web Service Output* module to the output of your *Evaluate Model* module.</span></span>

<span data-ttu-id="20f52-130">Kör experimentet.</span><span class="sxs-lookup"><span data-stu-id="20f52-130">Run your experiment.</span></span>

<span data-ttu-id="20f52-131">Därefter måste du distribuera utbildning experiment som en webbtjänst som producerar en tränad modell och utvärderingsresultat av modellen.</span><span class="sxs-lookup"><span data-stu-id="20f52-131">Next, you must deploy the training experiment as a web service that produces a trained model and model evaluation results.</span></span>  

<span data-ttu-id="20f52-132">Längst ned i arbetsytan för experimentet klickar du på **konfigurera Web Service**, och välj sedan **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="20f52-132">At the bottom of the experiment canvas, click **Set Up Web Service**, and then select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="20f52-133">Azure Machine Learning-webbtjänster portal öppnar den **distribuera webbtjänsten** sidan.</span><span class="sxs-lookup"><span data-stu-id="20f52-133">The Azure Machine Learning Web Services portal opens to the **Deploy Web Service** page.</span></span> <span data-ttu-id="20f52-134">Ange ett namn för webbtjänsten, väljer en betalningsplan och klicka sedan på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="20f52-134">Type a name for your web service, choose a payment plan, and then click **Deploy**.</span></span> <span data-ttu-id="20f52-135">Du kan bara använda Batch Execution-metoden för att skapa tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="20f52-135">You can only use the Batch Execution method for creating trained models.</span></span>

## <a name="retrain-the-model-with-new-data-by-using-bes"></a><span data-ttu-id="20f52-136">Träna om modellen med nya data med hjälp av BES</span><span class="sxs-lookup"><span data-stu-id="20f52-136">Retrain the model with new data by using BES</span></span>
<span data-ttu-id="20f52-137">I det här exemplet använder vi C# skapa omtränings programmet.</span><span class="sxs-lookup"><span data-stu-id="20f52-137">For this example, we're using C# to create the retraining application.</span></span> <span data-ttu-id="20f52-138">Du kan också använda Python eller R exempelkod för att utföra den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="20f52-138">You can also use Python or R sample code to accomplish this task.</span></span>

<span data-ttu-id="20f52-139">Att anropa omtränings-API: erna:</span><span class="sxs-lookup"><span data-stu-id="20f52-139">To call the retraining APIs:</span></span>

1. <span data-ttu-id="20f52-140">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="20f52-140">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="20f52-141">Logga in på portalen Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="20f52-141">Sign in to the Machine Learning Web Services portal.</span></span>
3. <span data-ttu-id="20f52-142">Klicka på den webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="20f52-142">Click the web service that you're working with.</span></span>
4. <span data-ttu-id="20f52-143">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="20f52-143">Click **Consume**.</span></span>
5. <span data-ttu-id="20f52-144">Längst ned i den **förbruka** sidan den **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="20f52-144">At the bottom of the **Consume** page, in the **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="20f52-145">Kopiera C# exempelkod för batchkörning och klistra in den i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="20f52-145">Copy the sample C# code for batch execution and paste it into the Program.cs file.</span></span> <span data-ttu-id="20f52-146">Kontrollera att namnområdet intakt.</span><span class="sxs-lookup"><span data-stu-id="20f52-146">Make sure that the namespace remains intact.</span></span>

<span data-ttu-id="20f52-147">Lägg till NuGet-paketet Microsoft.AspNet.WebApi.Client, som anges i kommentarerna.</span><span class="sxs-lookup"><span data-stu-id="20f52-147">Add the NuGet package Microsoft.AspNet.WebApi.Client, as specified in the comments.</span></span> <span data-ttu-id="20f52-148">Om du vill lägga till en referens till Microsoft.WindowsAzure.Storage.dll måste du kanske måste först installera den [klientbiblioteket för Azure Storage-tjänster](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="20f52-148">To add the reference to Microsoft.WindowsAzure.Storage.dll, you might first need to install the [client library for Azure Storage services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

<span data-ttu-id="20f52-149">I följande skärmbild visas den **förbruka** sida i Azure Machine Learning-webbtjänster-portalen.</span><span class="sxs-lookup"><span data-stu-id="20f52-149">The following screenshot shows the **Consume** page in the Azure Machine Learning Web Services portal.</span></span>

![Använda sidan][1]

### <a name="update-the-apikey-declaration"></a><span data-ttu-id="20f52-151">Uppdatera apikey deklarationen</span><span class="sxs-lookup"><span data-stu-id="20f52-151">Update the apikey declaration</span></span>
<span data-ttu-id="20f52-152">Leta upp den **apikey** deklaration:</span><span class="sxs-lookup"><span data-stu-id="20f52-152">Locate the **apikey** declaration:</span></span>

    const string apiKey = "abc123"; // Replace this with the API key for the web service

<span data-ttu-id="20f52-153">I den **grundläggande förbrukning info** avsnitt i den **förbruka** , leta upp den primära nyckeln och kopiera den till den **apikey** deklaration.</span><span class="sxs-lookup"><span data-stu-id="20f52-153">In the **Basic consumption info** section of the **Consume** page, locate the primary key and copy it to the **apikey** declaration.</span></span>

### <a name="update-the-azure-storage-information"></a><span data-ttu-id="20f52-154">Uppdatera Azure Storage-informationen</span><span class="sxs-lookup"><span data-stu-id="20f52-154">Update the Azure Storage information</span></span>
<span data-ttu-id="20f52-155">Exempelkoden BES överför en fil från en lokal enhet (till exempel ”C:\temp\CensusIpnput.csv”) till Azure Storage, bearbetar den och skriver resultatet till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="20f52-155">The BES sample code uploads a file from a local drive (for example, "C:\temp\CensusIpnput.csv") to Azure Storage, processes it, and writes the results back to Azure Storage.</span></span>  

<span data-ttu-id="20f52-156">Om du vill uppdatera Azure Storage-informationen måste du hämta lagringskontonamnet, behållare information och nyckeln för ditt lagringskonto från den klassiska Azure-portalen och sedan uppdatera correspondi när du har kört experimentet resulterande arbetsflödet bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="20f52-156">To update the Azure Storage information, you must retrieve the storage account name, key, and container information for your storage account from the Azure classic portal, and then update the correspondi After running your experiment, the resulting workflow should be similar to the following:</span></span>

![Resulterande arbetsflödet när kör][4]<span data-ttu-id="20f52-158">NG värden i koden.</span><span class="sxs-lookup"><span data-stu-id="20f52-158">ng values in the code.</span></span>

1. <span data-ttu-id="20f52-159">Logga in på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="20f52-159">Sign in to the Azure classic portal.</span></span>
2. <span data-ttu-id="20f52-160">Klicka på den vänstra navigeringsfönstret i kolumnen **lagring**.</span><span class="sxs-lookup"><span data-stu-id="20f52-160">In the left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="20f52-161">Välj en för att lagra retrained modellen från listan över storage-konton.</span><span class="sxs-lookup"><span data-stu-id="20f52-161">From the list of storage accounts, select one to store the retrained model.</span></span>
4. <span data-ttu-id="20f52-162">Längst ned på sidan klickar du på **hantera åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="20f52-162">At the bottom of the page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="20f52-163">Kopiera och spara den **primära åtkomstnyckeln** och Stäng dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="20f52-163">Copy and save the **Primary Access Key** and close the dialog.</span></span>
6. <span data-ttu-id="20f52-164">Överst på sidan, klickar du på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="20f52-164">At the top of the page, click **Containers**.</span></span>
7. <span data-ttu-id="20f52-165">Välj en befintlig behållare eller skapa en ny och spara namnet.</span><span class="sxs-lookup"><span data-stu-id="20f52-165">Select an existing container, or create a new one and save the name.</span></span>

<span data-ttu-id="20f52-166">Leta upp den *StorageAccountName*, *StorageAccountKey*, och *StorageContainerName* deklarationer, och uppdatera värden som du sparade från den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="20f52-166">Locate the *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations, and update the values that you saved from the classic portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

<span data-ttu-id="20f52-167">Du måste också kontrollera att filen finns på den plats som du anger i koden.</span><span class="sxs-lookup"><span data-stu-id="20f52-167">You also must ensure that the input file is available at the location that you specify in the code.</span></span>

### <a name="specify-the-output-location"></a><span data-ttu-id="20f52-168">Ange platsen för utdata</span><span class="sxs-lookup"><span data-stu-id="20f52-168">Specify the output location</span></span>
<span data-ttu-id="20f52-169">När du anger platsen i begäran nyttolasten tillägget på den fil som har angetts i *RelativeLocation* måste anges som `ilearner`.</span><span class="sxs-lookup"><span data-stu-id="20f52-169">When you specify the output location in the Request Payload, the extension of the file that is specified in *RelativeLocation* must be specified as `ilearner`.</span></span> <span data-ttu-id="20f52-170">Se följande exempel:</span><span class="sxs-lookup"><span data-stu-id="20f52-170">See the following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with the location you want to use for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

<span data-ttu-id="20f52-171">Följande är ett exempel på omtränings utdata: ![Omtränings utdata][6]</span><span class="sxs-lookup"><span data-stu-id="20f52-171">The following is an example of retraining output: ![Retraining output][6]</span></span>

## <a name="evaluate-the-retraining-results"></a><span data-ttu-id="20f52-172">Utvärdera omtränings resultaten</span><span class="sxs-lookup"><span data-stu-id="20f52-172">Evaluate the retraining results</span></span>
<span data-ttu-id="20f52-173">När du kör programmet innehåller utdata URL och delade signaturer åtkomsttoken som är nödvändiga för att komma åt utvärderingsresultaten.</span><span class="sxs-lookup"><span data-stu-id="20f52-173">When you run the application, the output includes the URL and shared access signatures token that are necessary to access the evaluation results.</span></span>

<span data-ttu-id="20f52-174">Du kan se prestandaresultat retrained modellen genom att kombinera den *BaseLocation*, *RelativeLocation*, och *SasBlobToken* från resultatet för *output2* (som visas i föregående omtränings utdata avbildningen) och klistra in den fullständiga URL: en i webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="20f52-174">You can see the performance results of the retrained model by combining the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results for *output2* (as shown in the preceding retraining output image) and pasting the complete URL into the browser address bar.</span></span>  

<span data-ttu-id="20f52-175">Granska resultaten för att avgöra om den nyligen tränade modellen utför bra för att ersätta den befintliga versionen.</span><span class="sxs-lookup"><span data-stu-id="20f52-175">Examine the results to determine whether the newly trained model performs well enough to replace the existing one.</span></span>

<span data-ttu-id="20f52-176">Kopiera den *BaseLocation*, *RelativeLocation*, och *SasBlobToken* från resultatet.</span><span class="sxs-lookup"><span data-stu-id="20f52-176">Copy the *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from the output results.</span></span>

## <a name="retrain-the-web-service"></a><span data-ttu-id="20f52-177">Träna om webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="20f52-177">Retrain the web service</span></span>
<span data-ttu-id="20f52-178">När du träna om en ny webbtjänst uppdatera förutsägande web service definition för att referera till den nya tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="20f52-178">When you retrain a new web service, you update the predictive web service definition to reference the new trained model.</span></span> <span data-ttu-id="20f52-179">Web service definition är en intern representation av den tränade modellen för webbtjänsten och kan inte ändras direkt.</span><span class="sxs-lookup"><span data-stu-id="20f52-179">The web service definition is an internal representation of the trained model of the web service and is not directly modifiable.</span></span> <span data-ttu-id="20f52-180">Kontrollera att du hämtar web service definition för experimentet förutsägbara och inte experimentet utbildning.</span><span class="sxs-lookup"><span data-stu-id="20f52-180">Make sure that you are retrieving the web service definition for your predictive experiment and not your training experiment.</span></span>

## <a name="sign-in-to-azure-resource-manager"></a><span data-ttu-id="20f52-181">Logga in till Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="20f52-181">Sign in to Azure Resource Manager</span></span>
<span data-ttu-id="20f52-182">Du måste först logga in på ditt Azure-konto från PowerShell-miljö med hjälp av den [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="20f52-182">You must first sign in to your Azure account from within the PowerShell environment by using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-the-web-service-definition-object"></a><span data-ttu-id="20f52-183">Hämta Web Service Definition objektet</span><span class="sxs-lookup"><span data-stu-id="20f52-183">Get the Web Service Definition object</span></span>
<span data-ttu-id="20f52-184">Hämta sedan Web Service Definition objektet genom att anropa den [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="20f52-184">Next, get the Web Service Definition object by calling the [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="20f52-185">För att fastställa resursgruppens namn för en befintlig webbtjänst, kör du cmdleten Get-AzureRmMlWebService utan några parametrar för att visa webbtjänsterna i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="20f52-185">To determine the resource group name of an existing web service, run the Get-AzureRmMlWebService cmdlet without any parameters to display the web services in your subscription.</span></span> <span data-ttu-id="20f52-186">Leta upp webbtjänsten och titta på dess web service-ID.</span><span class="sxs-lookup"><span data-stu-id="20f52-186">Locate the web service, and then look at its web service ID.</span></span> <span data-ttu-id="20f52-187">Namnet på resursgruppen är det fjärde elementet med ID precis efter den *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="20f52-187">The name of the resource group is the fourth element in the ID, just after the *resourceGroups* element.</span></span> <span data-ttu-id="20f52-188">I följande exempel är resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="20f52-188">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="20f52-189">Du kan också för att fastställa resursgruppens namn för en befintlig webbtjänst inloggning på portalen Azure Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="20f52-189">Alternatively, to determine the resource group name of an existing web service, sign in to the Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="20f52-190">Välj webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="20f52-190">Select the web service.</span></span> <span data-ttu-id="20f52-191">Resursgruppens namn är den femte del av URL för webbtjänsten precis efter den *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="20f52-191">The resource group name is the fifth element of the URL of the web service, just after the *resourceGroups* element.</span></span> <span data-ttu-id="20f52-192">I följande exempel är resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="20f52-192">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-object-as-json"></a><span data-ttu-id="20f52-193">Exportera Web Service Definition-objektet som JSON</span><span class="sxs-lookup"><span data-stu-id="20f52-193">Export the Web Service Definition object as JSON</span></span>
<span data-ttu-id="20f52-194">Om du vill ändra definitionen av den tränade modellen att använda den nya tränade modellen måste du först använda den [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) för att exportera den till en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="20f52-194">To modify the definition of the trained model to use the newly trained model, you must first use the [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet to export it to a JSON-format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob"></a><span data-ttu-id="20f52-195">Uppdatera referens till ilearner-blob</span><span class="sxs-lookup"><span data-stu-id="20f52-195">Update the reference to the ilearner blob</span></span>
<span data-ttu-id="20f52-196">Leta upp [tränad modell] i tillgångarna, uppdatera den *uri* värde i den *locationInfo* nod med ilearner-blob-URI.</span><span class="sxs-lookup"><span data-stu-id="20f52-196">In the assets, locate the [trained model], update the *uri* value in the *locationInfo* node with the URI of the ilearner blob.</span></span> <span data-ttu-id="20f52-197">URI: N skapas genom att kombinera den *BaseLocation* och *RelativeLocation* från utdata från BES omtränings anrop.</span><span class="sxs-lookup"><span data-stu-id="20f52-197">The URI is generated by combining the *BaseLocation* and the *RelativeLocation* from the output of the BES retraining call.</span></span>

     "asset3": {
        "name": "Retrain Sample [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition-object"></a><span data-ttu-id="20f52-198">Importera JSON till ett objekt för Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="20f52-198">Import the JSON into a Web Service Definition object</span></span>
<span data-ttu-id="20f52-199">Du måste använda den [importera AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) för att omvandla den ändrade JSON-filen tillbaka till Web Service Definition-objekt som du kan använda för att uppdatera predicative experimentet.</span><span class="sxs-lookup"><span data-stu-id="20f52-199">You must use the [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet to convert the modified JSON file back into a Web Service Definition object that you can use to update the predicative experiment.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service"></a><span data-ttu-id="20f52-200">Uppdatera webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="20f52-200">Update the web service</span></span>
<span data-ttu-id="20f52-201">Använd slutligen den [uppdatering AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) att uppdatera prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="20f52-201">Finally, use the [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet to update the predictive experiment.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
