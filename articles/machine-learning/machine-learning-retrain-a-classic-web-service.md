---
title: "aaaRetrain en klassisk webbtjänst | Microsoft Docs"
description: "Lär dig hur tooprogrammatically träna om en modell och uppdatera hello web service toouse hello nyligen trained model i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: d3ba21ed75f02868535cb2fcac607643303a9554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-classic-web-service"></a><span data-ttu-id="e6634-103">Omtrimma en klassisk webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e6634-103">Retrain a Classic web service</span></span>
<span data-ttu-id="e6634-104">hello förutsägande webbtjänst som du har distribuerat är hello standard bedömningsslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="e6634-104">hello Predictive Web Service you deployed is hello default scoring endpoint.</span></span> <span data-ttu-id="e6634-105">Slutpunkter hålls synkroniserade med hello ursprungliga träning och bedömningen experiment, och därför hello tränad modell för hello standardslutpunkten kan inte ersättas.</span><span class="sxs-lookup"><span data-stu-id="e6634-105">Default endpoints are kept in sync with hello original training and scoring experiments, and therefore hello trained model for hello default endpoint cannot be replaced.</span></span> <span data-ttu-id="e6634-106">webbtjänst för tooretrain hello, måste du lägga till en ny slutpunkt toohello-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="e6634-106">tooretrain hello web service, you must add a new endpoint toohello web service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e6634-107">Krav</span><span class="sxs-lookup"><span data-stu-id="e6634-107">Prerequisites</span></span>
<span data-ttu-id="e6634-108">Du måste ha konfigurerat en träningsexperiment och prediktivt experiment som visas i [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="e6634-108">You must have set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="e6634-109">Hej prediktivt experiment måste distribueras som klassisk machine learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="e6634-109">hello predictive experiment must be deployed as a Classic machine learning web service.</span></span> 
> 
> 

<span data-ttu-id="e6634-110">Ytterligare information om distribuera webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="e6634-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="add-a-new-endpoint"></a><span data-ttu-id="e6634-111">Lägg till en ny slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e6634-111">Add a new Endpoint</span></span>
<span data-ttu-id="e6634-112">hello förutsägande webbtjänst som du har distribuerat innehåller en bedömningsslutpunkten som hålls synkroniserade med hello ursprungliga utbildning och bedömningsprofil experiment tränad modell.</span><span class="sxs-lookup"><span data-stu-id="e6634-112">hello Predictive Web Service that you deployed contains a default scoring endpoint that is kept in sync with hello original training and scoring experiments trained model.</span></span> <span data-ttu-id="e6634-113">tooupdate din web service toowith en ny tränad modell, måste du skapa en ny bedömningsprofil slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e6634-113">tooupdate your web service toowith a new trained model, you must create a new scoring endpoint.</span></span> 

<span data-ttu-id="e6634-114">toocreate en ny bedömningsprofil slutpunkt på hello förutsägande webbtjänst som kan uppdateras med hello tränade modellen:</span><span class="sxs-lookup"><span data-stu-id="e6634-114">toocreate a new scoring endpoint, on hello Predictive Web Service that can be updated with hello trained model:</span></span>

> [!NOTE]
> <span data-ttu-id="e6634-115">Var noga med att du lägger till hello endpoint toohello förutsägande webbtjänst, inte hello utbildning webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="e6634-115">Be sure you are adding hello endpoint toohello Predictive Web Service, not hello Training Web Service.</span></span> <span data-ttu-id="e6634-116">Om du har distribuerat en utbildnings- och en förutsägbar webbtjänst korrekt, bör du se två separata webbtjänster som anges.</span><span class="sxs-lookup"><span data-stu-id="e6634-116">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate web services listed.</span></span> <span data-ttu-id="e6634-117">hello förutsägande webbtjänsten ska avslutas med ”[förutsägande exp.]”.</span><span class="sxs-lookup"><span data-stu-id="e6634-117">hello Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

<span data-ttu-id="e6634-118">Det finns tre sätt som du kan lägga till en ny slutpunkt tooa webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="e6634-118">There are three ways in which you can add a new end point tooa web service:</span></span>

1. <span data-ttu-id="e6634-119">Programmässigt</span><span class="sxs-lookup"><span data-stu-id="e6634-119">Programmatically</span></span>
2. <span data-ttu-id="e6634-120">Använd hello webbtjänster för Microsoft Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e6634-120">Use hello Microsoft Azure Web Services portal</span></span>
3. <span data-ttu-id="e6634-121">Använd hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="e6634-121">Use hello Azure classic portal</span></span>

### <a name="programmatically-add-an-endpoint"></a><span data-ttu-id="e6634-122">Lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e6634-122">Programmatically add an endpoint</span></span>
<span data-ttu-id="e6634-123">Du kan lägga till bedömningsprofil slutpunkter som använder hello exempelkoden i detta [github-lagringsplatsen](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="e6634-123">You can add scoring endpoints using hello sample code provided in this [github repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span></span>

### <a name="use-hello-microsoft-azure-web-services-portal-tooadd-an-endpoint"></a><span data-ttu-id="e6634-124">Använd hello webbtjänster för Microsoft Azure portal tooadd en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e6634-124">Use hello Microsoft Azure Web Services portal tooadd an endpoint</span></span>
1. <span data-ttu-id="e6634-125">Klicka på webbtjänster på hello navigeringen till vänster kolumn i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="e6634-125">In Machine Learning Studio, on hello left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="e6634-126">Hello längst ned på hello web service instrumentpanelen, klickar du på **hantera slutpunkter preview**.</span><span class="sxs-lookup"><span data-stu-id="e6634-126">At hello bottom of hello web service dashboard, click **Manage endpoints preview**.</span></span>
3. <span data-ttu-id="e6634-127">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e6634-127">Click **Add**.</span></span>
4. <span data-ttu-id="e6634-128">Skriv ett namn och beskrivning för hello ny slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e6634-128">Type a name and description for hello new endpoint.</span></span> <span data-ttu-id="e6634-129">Välj hello loggningsnivån och om exempeldata är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="e6634-129">Select hello logging level and whether sample data is enabled.</span></span> <span data-ttu-id="e6634-130">Mer information om loggning finns [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="e6634-130">For more information on logging, see [Enable logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>

### <a name="use-hello-azure-classic-portal-tooadd-an-endpoint"></a><span data-ttu-id="e6634-131">Använd hello Azure klassiska portal tooadd en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="e6634-131">Use hello Azure classic portal tooadd an endpoint</span></span>
1. <span data-ttu-id="e6634-132">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="e6634-132">Sign in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="e6634-133">Hello vänstra menyn klickar du på **Maskininlärning**.</span><span class="sxs-lookup"><span data-stu-id="e6634-133">In hello left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="e6634-134">Klicka på arbetsytan under namn och klicka sedan på **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="e6634-134">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="e6634-135">Under namn, klickar du på **inventering modellen [förutsägande exp].** .</span><span class="sxs-lookup"><span data-stu-id="e6634-135">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="e6634-136">Hello längst hello-sidan, klickar du på **Lägg till slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="e6634-136">At hello bottom of hello page, click **Add Endpoint**.</span></span> <span data-ttu-id="e6634-137">Mer information om att lägga till slutpunkter finns [skapar slutpunkter](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e6634-137">For more information on adding endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span> 

## <a name="update-hello-added-endpoints-trained-model"></a><span data-ttu-id="e6634-138">Uppdatera hello läggs slutpunktens Trained Model</span><span class="sxs-lookup"><span data-stu-id="e6634-138">Update hello added endpoint’s Trained Model</span></span>
<span data-ttu-id="e6634-139">toocomplete hello omtränings-processen, måste du uppdatera hello tränad modell för hello ny slutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="e6634-139">toocomplete hello retraining process, you must update hello trained model of hello new endpoint that you added.</span></span>

* <span data-ttu-id="e6634-140">Om du har lagt till hello ny slutpunkt med hello klassiska Azure-portalen kan du klicka på hello ny slutpunkt i hello-portalen och sedan hello **UpdateResource** länka tooget hello URL måste tooupdate hello endpoint modellen.</span><span class="sxs-lookup"><span data-stu-id="e6634-140">If you added hello new endpoint using hello classic Azure portal, you can click hello new endpoint's name in hello portal, then hello **UpdateResource** link tooget hello URL you would need tooupdate hello endpoint's model.</span></span>
* <span data-ttu-id="e6634-141">Om du har lagt till hello slutpunkten med hjälp av hello exempelkod inkluderar platsen för hello Hjälpsidans URL som identifieras av hello *HelpLocationURL* värdet i hello utdata.</span><span class="sxs-lookup"><span data-stu-id="e6634-141">If you added hello endpoint using hello sample code, this includes location of hello help URL identified by hello *HelpLocationURL* value in hello output.</span></span>

<span data-ttu-id="e6634-142">URL till tooretrieve hello-sökväg:</span><span class="sxs-lookup"><span data-stu-id="e6634-142">tooretrieve hello path URL:</span></span>

1. <span data-ttu-id="e6634-143">Kopiera och klistra in hello URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="e6634-143">Copy and paste hello URL into your browser.</span></span>
2. <span data-ttu-id="e6634-144">Klicka på länken för hello Update resurs.</span><span class="sxs-lookup"><span data-stu-id="e6634-144">Click hello Update Resource link.</span></span>
3. <span data-ttu-id="e6634-145">Kopiera hello POST URL för hello PATCH-begäran.</span><span class="sxs-lookup"><span data-stu-id="e6634-145">Copy hello POST URL of hello PATCH request.</span></span> <span data-ttu-id="e6634-146">Exempel:</span><span class="sxs-lookup"><span data-stu-id="e6634-146">For example:</span></span>
   
     <span data-ttu-id="e6634-147">KORRIGERING AV URL: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2</span><span class="sxs-lookup"><span data-stu-id="e6634-147">PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2</span></span>

<span data-ttu-id="e6634-148">Du kan nu använda hello tränade modellen tooupdate hello bedömningsslutpunkten som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e6634-148">You can now use hello trained model tooupdate hello scoring endpoint that you created previously.</span></span>

<span data-ttu-id="e6634-149">hello följande exempelkod visar hur toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, och KORRIGERA URL tooupdate hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="e6634-149">hello following sample code shows you how toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, and PATCH URL tooupdate hello endpoint.</span></span>

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from hello output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from hello output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

<span data-ttu-id="e6634-150">Hej *apiKey* och hello *endpointUrl* för hello-anropet kan hämtas från slutpunkten instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e6634-150">hello *apiKey* and hello *endpointUrl* for hello call can be obtained from endpoint dashboard.</span></span>

<span data-ttu-id="e6634-151">Hej värdet för hello *namn* parameter i *resurser* bör matcha hello resursnamn av hello sparas Trained Model i hello prediktivt experiment.</span><span class="sxs-lookup"><span data-stu-id="e6634-151">hello value of hello *Name* parameter in *Resources* should match hello Resource Name of hello saved Trained Model in hello predictive experiment.</span></span> <span data-ttu-id="e6634-152">tooget hello resursnamn:</span><span class="sxs-lookup"><span data-stu-id="e6634-152">tooget hello Resource Name:</span></span>

1. <span data-ttu-id="e6634-153">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="e6634-153">Sign in toohello [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="e6634-154">Hello vänstra menyn klickar du på **Maskininlärning**.</span><span class="sxs-lookup"><span data-stu-id="e6634-154">In hello left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="e6634-155">Klicka på arbetsytan under namn och klicka sedan på **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="e6634-155">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="e6634-156">Under namn, klickar du på **inventering modellen [förutsägande exp].** .</span><span class="sxs-lookup"><span data-stu-id="e6634-156">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="e6634-157">Klicka på ny hello slutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="e6634-157">Click hello new endpoint you added.</span></span>
6. <span data-ttu-id="e6634-158">Hello endpoint instrumentpanelen, klicka på **uppdatering resurs**.</span><span class="sxs-lookup"><span data-stu-id="e6634-158">On hello endpoint dashboard, click **Update Resource**.</span></span>
7. <span data-ttu-id="e6634-159">Hello Update resurs API-dokumentationen sidan för hello webbtjänsten hittar hello **resursnamnet** under **uppdateras resurser**.</span><span class="sxs-lookup"><span data-stu-id="e6634-159">On hello Update Resource API Documentation page for hello web service, you can find hello **Resource Name** under **Updatable Resources**.</span></span>

<span data-ttu-id="e6634-160">Om din SAS-token upphör att gälla innan du har uppdaterat hello slutpunkt, måste du utföra GET med hello jobb-Id tooobtain en ny token.</span><span class="sxs-lookup"><span data-stu-id="e6634-160">If your SAS token expires before you finish updating hello endpoint, you must perform a GET with hello Job Id tooobtain a fresh token.</span></span>

<span data-ttu-id="e6634-161">När hello koden har körts ska hello ny slutpunkt börja använda hello retrained modellen i cirka 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="e6634-161">When hello code has successfully run, hello new endpoint should start using hello retrained model in approximately 30 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="e6634-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e6634-162">Summary</span></span>
<span data-ttu-id="e6634-163">Med hjälp av Hej Omtränings-API: er, kan du uppdatera hello tränad modell för en förutsägbar webbtjänst som aktiverar scenarier:</span><span class="sxs-lookup"><span data-stu-id="e6634-163">Using hello Retraining APIs, you can update hello trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="e6634-164">Periodiska modell via programmering med nya data.</span><span class="sxs-lookup"><span data-stu-id="e6634-164">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="e6634-165">Distribution av en modell toocustomers med hello målet för att låta dem träna om hello modellen med sina egna data.</span><span class="sxs-lookup"><span data-stu-id="e6634-165">Distribution of a model toocustomers with hello goal of letting them retrain hello model using their own data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e6634-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e6634-166">Next steps</span></span>
[<span data-ttu-id="e6634-167">Felsöka hello omtränings av en klassisk Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e6634-167">Troubleshooting hello retraining of an Azure Machine Learning classic web service</span></span>](machine-learning-troubleshooting-retraining-models.md)

