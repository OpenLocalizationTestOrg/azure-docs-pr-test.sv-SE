---
title: "en befintlig förutsägande aaaRetrain webbtjänsten | Microsoft Docs"
description: "Lär dig hur tooretrain en modell och uppdatera hello web service toouse hello nyligen tränade modellen i Azure Machine Learning."
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
ms.openlocfilehash: fb0760d0a2adc34fc5f3df1ae41bdac075f91bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a><span data-ttu-id="d6df9-103">Träna om en befintlig förutsägande webbtjänst</span><span class="sxs-lookup"><span data-stu-id="d6df9-103">Retrain an existing predictive web service</span></span>
<span data-ttu-id="d6df9-104">Det här dokumentet beskriver hello omtränings-process för hello följande scenario:</span><span class="sxs-lookup"><span data-stu-id="d6df9-104">This document describes hello retraining process for hello following scenario:</span></span>

* <span data-ttu-id="d6df9-105">Du har en träningsexperiment och en prediktivt experiment som du har distribuerat som en operationalized webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d6df9-105">You have a training experiment and a predictive experiment that you have deployed as an operationalized web service.</span></span>
* <span data-ttu-id="d6df9-106">Du har nya data som du vill att din förutsägande web service toouse tooperform dess bedömningen.</span><span class="sxs-lookup"><span data-stu-id="d6df9-106">You have new data that you want your predictive web service toouse tooperform its scoring.</span></span>

> [!NOTE] 
> <span data-ttu-id="d6df9-107">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d6df9-107">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="d6df9-108">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="d6df9-108">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="d6df9-109">Från och med din befintliga webbtjänsten och experiment, behöver du toofollow de här stegen:</span><span class="sxs-lookup"><span data-stu-id="d6df9-109">Starting with your existing web service and experiments, you need toofollow these steps:</span></span>

1. <span data-ttu-id="d6df9-110">Uppdatera hello modellen.</span><span class="sxs-lookup"><span data-stu-id="d6df9-110">Update hello model.</span></span>
   1. <span data-ttu-id="d6df9-111">Ändra din utbildning experiment tooallow för web service in- och utdataenheter.</span><span class="sxs-lookup"><span data-stu-id="d6df9-111">Modify your training experiment tooallow for web service inputs and outputs.</span></span>
   2. <span data-ttu-id="d6df9-112">Distribuera hello träningsexperiment som en omtränings-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d6df9-112">Deploy hello training experiment as a retraining web service.</span></span>
   3. <span data-ttu-id="d6df9-113">Använda hello utbildning experiment Batch Execution Service BES-tooretrain hello modellen.</span><span class="sxs-lookup"><span data-stu-id="d6df9-113">Use hello training experiment's Batch Execution Service (BES) tooretrain hello model.</span></span>
2. <span data-ttu-id="d6df9-114">Använd hello Azure Machine Learning PowerShell-cmdlets tooupdate hello prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="d6df9-114">Use hello Azure Machine Learning PowerShell cmdlets tooupdate hello predictive experiment.</span></span>
   1. <span data-ttu-id="d6df9-115">Logga in tooyour Azure Resource Manager-konto.</span><span class="sxs-lookup"><span data-stu-id="d6df9-115">Sign in tooyour Azure Resource Manager account.</span></span>
   2. <span data-ttu-id="d6df9-116">Hämta hello web service definition.</span><span class="sxs-lookup"><span data-stu-id="d6df9-116">Get hello web service definition.</span></span>
   3. <span data-ttu-id="d6df9-117">Exportera hello web service definition som JSON.</span><span class="sxs-lookup"><span data-stu-id="d6df9-117">Export hello web service definition as JSON.</span></span>
   4. <span data-ttu-id="d6df9-118">Uppdatera hello toohello ilearner referensblobben i hello JSON.</span><span class="sxs-lookup"><span data-stu-id="d6df9-118">Update hello reference toohello ilearner blob in hello JSON.</span></span>
   5. <span data-ttu-id="d6df9-119">Importera hello JSON till en web service definition.</span><span class="sxs-lookup"><span data-stu-id="d6df9-119">Import hello JSON into a web service definition.</span></span>
   6. <span data-ttu-id="d6df9-120">Uppdatera hello-webbtjänsten med en ny web service definition.</span><span class="sxs-lookup"><span data-stu-id="d6df9-120">Update hello web service with a new web service definition.</span></span>

## <a name="deploy-hello-training-experiment"></a><span data-ttu-id="d6df9-121">Distribuera hello träningsexperiment</span><span class="sxs-lookup"><span data-stu-id="d6df9-121">Deploy hello training experiment</span></span>
<span data-ttu-id="d6df9-122">toodeploy hello träningsexperiment som en omtränings webbtjänst, måste du lägga till web service in- och utdataenheter toohello modell.</span><span class="sxs-lookup"><span data-stu-id="d6df9-122">toodeploy hello training experiment as a retraining web service, you must add web service inputs and outputs toohello model.</span></span> <span data-ttu-id="d6df9-123">Genom att ansluta en *Web Service utdata* modulen toohello experiment  *[Träningsmodell] [ train-model]*  modulen, aktivera hello utbildning experiment tooproduce en tränad modell som du kan använda i experimentet förutsägbara.</span><span class="sxs-lookup"><span data-stu-id="d6df9-123">By connecting a *Web Service Output* module toohello experiment's *[Train Model][train-model]* module, you enable hello training experiment tooproduce a new trained model that you can use in your predictive experiment.</span></span> <span data-ttu-id="d6df9-124">Om du har en *utvärdera modell* modulen, du kan även bifoga web service utdata tooget hello utvärderingsresultaten som utdata.</span><span class="sxs-lookup"><span data-stu-id="d6df9-124">If you have an *Evaluate Model* module, you can also attach web service output tooget hello evaluation results as output.</span></span>

<span data-ttu-id="d6df9-125">tooupdate experimentet utbildning:</span><span class="sxs-lookup"><span data-stu-id="d6df9-125">tooupdate your training experiment:</span></span>

1. <span data-ttu-id="d6df9-126">Ansluta en *Web Service inkommande* modulen tooyour indata (till exempel en *Rensa Data som saknas* module).</span><span class="sxs-lookup"><span data-stu-id="d6df9-126">Connect a *Web Service Input* module tooyour data input (for example, a *Clean Missing Data* module).</span></span> <span data-ttu-id="d6df9-127">Vanligtvis vill du tooensure som bearbetas i dina indata hello samma sätt som den ursprungliga informationen för utbildning.</span><span class="sxs-lookup"><span data-stu-id="d6df9-127">Typically, you want tooensure that your input data is processed in hello same way as your original training data.</span></span>
2. <span data-ttu-id="d6df9-128">Ansluta en *Web Service utdata* modulen toohello utdata från din *Träningsmodell* modul.</span><span class="sxs-lookup"><span data-stu-id="d6df9-128">Connect a *Web Service Output* module toohello output of your *Train Model* module.</span></span>
3. <span data-ttu-id="d6df9-129">Om du har en *utvärdera modell* modul och du vill att toooutput hello utvärderingsresultaten kan ansluta en *Web Service utdata* modulen toohello utdata från din *utvärdera modell* modul.</span><span class="sxs-lookup"><span data-stu-id="d6df9-129">If you have an *Evaluate Model* module and you want toooutput hello evaluation results, connect a *Web Service Output* module toohello output of your *Evaluate Model* module.</span></span>

<span data-ttu-id="d6df9-130">Kör experimentet.</span><span class="sxs-lookup"><span data-stu-id="d6df9-130">Run your experiment.</span></span>

<span data-ttu-id="d6df9-131">Därefter måste du distribuera hello träningsexperiment som en webbtjänst som producerar en tränad modell och utvärderingsresultat av modellen.</span><span class="sxs-lookup"><span data-stu-id="d6df9-131">Next, you must deploy hello training experiment as a web service that produces a trained model and model evaluation results.</span></span>  

<span data-ttu-id="d6df9-132">Hello längst ned på hello experimentet klickar du på **konfigurera Web Service**, och välj sedan **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-132">At hello bottom of hello experiment canvas, click **Set Up Web Service**, and then select **Deploy Web Service [New]**.</span></span> <span data-ttu-id="d6df9-133">hello Azure Machine Learning-webbtjänster portalen öppnar toohello **distribuera webbtjänsten** sidan.</span><span class="sxs-lookup"><span data-stu-id="d6df9-133">hello Azure Machine Learning Web Services portal opens toohello **Deploy Web Service** page.</span></span> <span data-ttu-id="d6df9-134">Ange ett namn för webbtjänsten, väljer en betalningsplan och klicka sedan på **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-134">Type a name for your web service, choose a payment plan, and then click **Deploy**.</span></span> <span data-ttu-id="d6df9-135">Du kan bara använda hello Batch Execution metod för att skapa tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="d6df9-135">You can only use hello Batch Execution method for creating trained models.</span></span>

## <a name="retrain-hello-model-with-new-data-by-using-bes"></a><span data-ttu-id="d6df9-136">Träna om hello modellen med nya data med hjälp av BES</span><span class="sxs-lookup"><span data-stu-id="d6df9-136">Retrain hello model with new data by using BES</span></span>
<span data-ttu-id="d6df9-137">I det här exemplet använder vi C# toocreate hello omtränings program.</span><span class="sxs-lookup"><span data-stu-id="d6df9-137">For this example, we're using C# toocreate hello retraining application.</span></span> <span data-ttu-id="d6df9-138">Du kan också använda Python eller R exempel kod tooaccomplish den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="d6df9-138">You can also use Python or R sample code tooaccomplish this task.</span></span>

<span data-ttu-id="d6df9-139">toocall hello omtränings-API: er:</span><span class="sxs-lookup"><span data-stu-id="d6df9-139">toocall hello retraining APIs:</span></span>

1. <span data-ttu-id="d6df9-140">Skapa ett C#-konsolprogram i Visual Studio: **ny** > **projekt** > **Visual C#** > **Windows Klassiska Desktop** > **konsolen App (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-140">Create a C# console application in Visual Studio: **New** > **Project** > **Visual C#** > **Windows Classic Desktop** > **Console App (.NET Framework)**.</span></span>
2. <span data-ttu-id="d6df9-141">Logga in toohello Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="d6df9-141">Sign in toohello Machine Learning Web Services portal.</span></span>
3. <span data-ttu-id="d6df9-142">Klicka på hello-webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="d6df9-142">Click hello web service that you're working with.</span></span>
4. <span data-ttu-id="d6df9-143">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-143">Click **Consume**.</span></span>
5. <span data-ttu-id="d6df9-144">Längst ned hello hello **förbruka** i hello sidan **exempelkod** klickar du på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-144">At hello bottom of hello **Consume** page, in hello **Sample Code** section, click **Batch**.</span></span>
6. <span data-ttu-id="d6df9-145">Kopiera hello C# exempelkod för batchkörning och klistra in den i hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="d6df9-145">Copy hello sample C# code for batch execution and paste it into hello Program.cs file.</span></span> <span data-ttu-id="d6df9-146">Kontrollera att hello namnutrymmet intakt.</span><span class="sxs-lookup"><span data-stu-id="d6df9-146">Make sure that hello namespace remains intact.</span></span>

<span data-ttu-id="d6df9-147">Lägg till hello NuGet-paketet Microsoft.AspNet.WebApi.Client, som anges i hello kommentarer.</span><span class="sxs-lookup"><span data-stu-id="d6df9-147">Add hello NuGet package Microsoft.AspNet.WebApi.Client, as specified in hello comments.</span></span> <span data-ttu-id="d6df9-148">tooadd hello referens tooMicrosoft.WindowsAzure.Storage.dll måste du eventuellt tooinstall hello [klientbiblioteket för Azure Storage-tjänster](https://www.nuget.org/packages/WindowsAzure.Storage).</span><span class="sxs-lookup"><span data-stu-id="d6df9-148">tooadd hello reference tooMicrosoft.WindowsAzure.Storage.dll, you might first need tooinstall hello [client library for Azure Storage services](https://www.nuget.org/packages/WindowsAzure.Storage).</span></span>

<span data-ttu-id="d6df9-149">hello följande skärmbild visar hello **förbruka** sida i portalen för hello Azure Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="d6df9-149">hello following screenshot shows hello **Consume** page in hello Azure Machine Learning Web Services portal.</span></span>

![Använda sidan][1]

### <a name="update-hello-apikey-declaration"></a><span data-ttu-id="d6df9-151">Uppdatera hello apikey förklaring</span><span class="sxs-lookup"><span data-stu-id="d6df9-151">Update hello apikey declaration</span></span>
<span data-ttu-id="d6df9-152">Leta upp hello **apikey** deklaration:</span><span class="sxs-lookup"><span data-stu-id="d6df9-152">Locate hello **apikey** declaration:</span></span>

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

<span data-ttu-id="d6df9-153">I hello **grundläggande förbrukning info** avsnitt i hello **förbruka** , leta upp hello primärnyckel och kopiera den toohello **apikey** deklaration.</span><span class="sxs-lookup"><span data-stu-id="d6df9-153">In hello **Basic consumption info** section of hello **Consume** page, locate hello primary key and copy it toohello **apikey** declaration.</span></span>

### <a name="update-hello-azure-storage-information"></a><span data-ttu-id="d6df9-154">Uppdatera hello Azure Storage-informationen</span><span class="sxs-lookup"><span data-stu-id="d6df9-154">Update hello Azure Storage information</span></span>
<span data-ttu-id="d6df9-155">Överför en fil från en lokal enhet (till exempel ”C:\temp\CensusIpnput.csv”) tooAzure lagring hello BES exempelkod, bearbetar den och skriver hello resultaten tillbaka tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="d6df9-155">hello BES sample code uploads a file from a local drive (for example, "C:\temp\CensusIpnput.csv") tooAzure Storage, processes it, and writes hello results back tooAzure Storage.</span></span>  

<span data-ttu-id="d6df9-156">tooupdate hello Azure Storage-informationen måste du hämta namn och nyckel behållaren information för ditt lagringskonto från hello klassiska Azure-portalen och sedan uppdatera hello correspondi när du har kört experimentet, hello resulterande hello storage-konto arbetsflödet ska vara liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="d6df9-156">tooupdate hello Azure Storage information, you must retrieve hello storage account name, key, and container information for your storage account from hello Azure classic portal, and then update hello correspondi After running your experiment, hello resulting workflow should be similar toohello following:</span></span>

![Resulterande arbetsflödet när kör][4]<span data-ttu-id="d6df9-158">NG värden i hello kod.</span><span class="sxs-lookup"><span data-stu-id="d6df9-158">ng values in hello code.</span></span>

1. <span data-ttu-id="d6df9-159">Logga in toohello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6df9-159">Sign in toohello Azure classic portal.</span></span>
2. <span data-ttu-id="d6df9-160">Klicka på hello navigeringen till vänster i kolumnen **lagring**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-160">In hello left navigation column, click **Storage**.</span></span>
3. <span data-ttu-id="d6df9-161">Välj en toostore hello retrained hello listan över storage-konton och modell.</span><span class="sxs-lookup"><span data-stu-id="d6df9-161">From hello list of storage accounts, select one toostore hello retrained model.</span></span>
4. <span data-ttu-id="d6df9-162">Hello längst hello-sidan, klickar du på **hantera åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-162">At hello bottom of hello page, click **Manage Access Keys**.</span></span>
5. <span data-ttu-id="d6df9-163">Kopiera och spara hello **primära åtkomstnyckeln** och Stäng hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d6df9-163">Copy and save hello **Primary Access Key** and close hello dialog.</span></span>
6. <span data-ttu-id="d6df9-164">Hello överst på hello-sidan, klickar du på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="d6df9-164">At hello top of hello page, click **Containers**.</span></span>
7. <span data-ttu-id="d6df9-165">Välj en befintlig behållare eller skapa en ny och spara hello namn.</span><span class="sxs-lookup"><span data-stu-id="d6df9-165">Select an existing container, or create a new one and save hello name.</span></span>

<span data-ttu-id="d6df9-166">Leta upp hello *StorageAccountName*, *StorageAccountKey*, och *StorageContainerName* deklarationer och uppdatera hello värden som du sparade från hello klassiska portalen .</span><span class="sxs-lookup"><span data-stu-id="d6df9-166">Locate hello *StorageAccountName*, *StorageAccountKey*, and *StorageContainerName* declarations, and update hello values that you saved from hello classic portal.</span></span>

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

<span data-ttu-id="d6df9-167">Du måste också kontrollera att hello indatafilen är tillgänglig på hello-plats som du anger i hello kod.</span><span class="sxs-lookup"><span data-stu-id="d6df9-167">You also must ensure that hello input file is available at hello location that you specify in hello code.</span></span>

### <a name="specify-hello-output-location"></a><span data-ttu-id="d6df9-168">Ange hello platsen</span><span class="sxs-lookup"><span data-stu-id="d6df9-168">Specify hello output location</span></span>
<span data-ttu-id="d6df9-169">När du anger hello platsen i hello begära nyttolast hello-tillägget för hello-filen som anges i *RelativeLocation* måste anges som `ilearner`.</span><span class="sxs-lookup"><span data-stu-id="d6df9-169">When you specify hello output location in hello Request Payload, hello extension of hello file that is specified in *RelativeLocation* must be specified as `ilearner`.</span></span> <span data-ttu-id="d6df9-170">Se följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="d6df9-170">See hello following example:</span></span>

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you want toouse for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

<span data-ttu-id="d6df9-171">hello följande är ett exempel på omtränings utdata: ![Omtränings utdata][6]</span><span class="sxs-lookup"><span data-stu-id="d6df9-171">hello following is an example of retraining output: ![Retraining output][6]</span></span>

## <a name="evaluate-hello-retraining-results"></a><span data-ttu-id="d6df9-172">Utvärdera hello omtränings resultat</span><span class="sxs-lookup"><span data-stu-id="d6df9-172">Evaluate hello retraining results</span></span>
<span data-ttu-id="d6df9-173">När du kör programmet hello innehåller hello utdata hello URL och delade signaturer åtkomsttoken som är nödvändiga tooaccess hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="d6df9-173">When you run hello application, hello output includes hello URL and shared access signatures token that are necessary tooaccess hello evaluation results.</span></span>

<span data-ttu-id="d6df9-174">Du kan se hello prestandaresultat hello retrained modellen genom att kombinera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* hello utdata resultaten för *output2* (som visas i föregående omtränings utdata avbildningen hello) och klistra in hello fullständiga URL: en i hello webbläsarens adressfält.</span><span class="sxs-lookup"><span data-stu-id="d6df9-174">You can see hello performance results of hello retrained model by combining hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results for *output2* (as shown in hello preceding retraining output image) and pasting hello complete URL into hello browser address bar.</span></span>  

<span data-ttu-id="d6df9-175">Undersöka hello resultat toodetermine om hello nyligen utbildade modellen presterar bra tillräckligt med tooreplace hello befintliga en.</span><span class="sxs-lookup"><span data-stu-id="d6df9-175">Examine hello results toodetermine whether hello newly trained model performs well enough tooreplace hello existing one.</span></span>

<span data-ttu-id="d6df9-176">Kopiera hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken* hello utdata resultaten.</span><span class="sxs-lookup"><span data-stu-id="d6df9-176">Copy hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken* from hello output results.</span></span>

## <a name="retrain-hello-web-service"></a><span data-ttu-id="d6df9-177">Träna om hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="d6df9-177">Retrain hello web service</span></span>
<span data-ttu-id="d6df9-178">När du träna om en ny webbtjänst uppdatera hello förutsägande web service definition tooreference hello ny tränad modell.</span><span class="sxs-lookup"><span data-stu-id="d6df9-178">When you retrain a new web service, you update hello predictive web service definition tooreference hello new trained model.</span></span> <span data-ttu-id="d6df9-179">hello web service definition är en intern representation av hello tränad modell för hello webbtjänsten och kan inte ändras direkt.</span><span class="sxs-lookup"><span data-stu-id="d6df9-179">hello web service definition is an internal representation of hello trained model of hello web service and is not directly modifiable.</span></span> <span data-ttu-id="d6df9-180">Kontrollera att du hämtar hello web service definition för experimentet förutsägbara och inte experimentet utbildning.</span><span class="sxs-lookup"><span data-stu-id="d6df9-180">Make sure that you are retrieving hello web service definition for your predictive experiment and not your training experiment.</span></span>

## <a name="sign-in-tooazure-resource-manager"></a><span data-ttu-id="d6df9-181">Logga in tooAzure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="d6df9-181">Sign in tooAzure Resource Manager</span></span>
<span data-ttu-id="d6df9-182">Du måste först loggar in tooyour Azure-konto från inom hello PowerShell-miljö med hjälp av hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d6df9-182">You must first sign in tooyour Azure account from within hello PowerShell environment by using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-hello-web-service-definition-object"></a><span data-ttu-id="d6df9-183">Hämta hello Web Service Definition objekt</span><span class="sxs-lookup"><span data-stu-id="d6df9-183">Get hello Web Service Definition object</span></span>
<span data-ttu-id="d6df9-184">Hämta sedan hello Web Service Definition objekt genom att anropa hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="d6df9-184">Next, get hello Web Service Definition object by calling hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="d6df9-185">toodetermine hello resursgruppens namn för en befintlig webbtjänst kör hello Get-AzureRmMlWebService cmdlet utan några parametrar toodisplay hello web-tjänster i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d6df9-185">toodetermine hello resource group name of an existing web service, run hello Get-AzureRmMlWebService cmdlet without any parameters toodisplay hello web services in your subscription.</span></span> <span data-ttu-id="d6df9-186">Leta upp hello webbtjänsten och titta på dess web service-ID.</span><span class="sxs-lookup"><span data-stu-id="d6df9-186">Locate hello web service, and then look at its web service ID.</span></span> <span data-ttu-id="d6df9-187">hello hello resursgruppen heter hello fjärde elementet i hello ID precis efter hello *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="d6df9-187">hello name of hello resource group is hello fourth element in hello ID, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="d6df9-188">I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="d6df9-188">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="d6df9-189">Du kan också toodetermine hello resursgruppens namn för en befintlig webbtjänst, logga in toohello Azure Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="d6df9-189">Alternatively, toodetermine hello resource group name of an existing web service, sign in toohello Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="d6df9-190">Välj hello-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="d6df9-190">Select hello web service.</span></span> <span data-ttu-id="d6df9-191">hello resursgruppens namn används hello femte element av hello URL för webbtjänsten för hello, bara efter hello *resursgrupper* element.</span><span class="sxs-lookup"><span data-stu-id="d6df9-191">hello resource group name is hello fifth element of hello URL of hello web service, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="d6df9-192">I följande exempel hello, används hello resursgruppens namn standard-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="d6df9-192">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-object-as-json"></a><span data-ttu-id="d6df9-193">Exportera hello Web Service Definition objekt som JSON</span><span class="sxs-lookup"><span data-stu-id="d6df9-193">Export hello Web Service Definition object as JSON</span></span>
<span data-ttu-id="d6df9-194">toomodify hello definition av hello tränade modellen toouse hello nyligen tränat modellen måste du först använda hello [Export AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport den filen tooa JSON-format.</span><span class="sxs-lookup"><span data-stu-id="d6df9-194">toomodify hello definition of hello trained model toouse hello newly trained model, you must first use hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport it tooa JSON-format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob"></a><span data-ttu-id="d6df9-195">Uppdatera hello referensblobben toohello ilearner</span><span class="sxs-lookup"><span data-stu-id="d6df9-195">Update hello reference toohello ilearner blob</span></span>
<span data-ttu-id="d6df9-196">Leta upp hello [tränad modell], uppdatering hello i hello tillgångar *uri* värdet i hello *locationInfo* nod med hello hello ilearner blob-URI.</span><span class="sxs-lookup"><span data-stu-id="d6df9-196">In hello assets, locate hello [trained model], update hello *uri* value in hello *locationInfo* node with hello URI of hello ilearner blob.</span></span> <span data-ttu-id="d6df9-197">hello URI genereras genom att kombinera hello *BaseLocation* och hello *RelativeLocation* från hello utdata från hello BES omtränings-anrop.</span><span class="sxs-lookup"><span data-stu-id="d6df9-197">hello URI is generated by combining hello *BaseLocation* and hello *RelativeLocation* from hello output of hello BES retraining call.</span></span>

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

## <a name="import-hello-json-into-a-web-service-definition-object"></a><span data-ttu-id="d6df9-198">Importera hello JSON till ett objekt för Web Service Definition</span><span class="sxs-lookup"><span data-stu-id="d6df9-198">Import hello JSON into a Web Service Definition object</span></span>
<span data-ttu-id="d6df9-199">Du måste använda hello [importera AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello ändrade JSON-fil i ett Web Service Definition-objekt som du kan använda tooupdate hello predicative experiment.</span><span class="sxs-lookup"><span data-stu-id="d6df9-199">You must use hello [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello modified JSON file back into a Web Service Definition object that you can use tooupdate hello predicative experiment.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service"></a><span data-ttu-id="d6df9-200">Uppdatera hello-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="d6df9-200">Update hello web service</span></span>
<span data-ttu-id="d6df9-201">Använd slutligen hello [uppdatering AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="d6df9-201">Finally, use hello [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello predictive experiment.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
