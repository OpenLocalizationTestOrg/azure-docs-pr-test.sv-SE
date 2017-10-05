---
title: "Träna om en klassisk webbtjänst | Microsoft Docs"
description: "Lär dig mer om att träna om en modell och uppdatera webbtjänsten för att använda den nya tränade modellen i Azure Machine Learning programmässigt."
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
ms.openlocfilehash: 3f9f4cd5ed36262845f7a3139073ccd4e49f472a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-classic-web-service"></a><span data-ttu-id="2f2e4-103">Omtrimma en klassisk webbtjänst</span><span class="sxs-lookup"><span data-stu-id="2f2e4-103">Retrain a Classic web service</span></span>
<span data-ttu-id="2f2e4-104">Förutsägande webbtjänsten som du har distribuerat är standard bedömningsslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-104">The Predictive Web Service you deployed is the default scoring endpoint.</span></span> <span data-ttu-id="2f2e4-105">Slutpunkter hålls synkroniserade med den ursprungliga utbildningen och bedömningen experiment och därför den tränade modellen för standardslutpunkten kan inte ersättas.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-105">Default endpoints are kept in sync with the original training and scoring experiments, and therefore the trained model for the default endpoint cannot be replaced.</span></span> <span data-ttu-id="2f2e4-106">För att träna om webbtjänsten måste du lägga till en ny slutpunkt till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-106">To retrain the web service, you must add a new endpoint to the web service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2f2e4-107">Krav</span><span class="sxs-lookup"><span data-stu-id="2f2e4-107">Prerequisites</span></span>
<span data-ttu-id="2f2e4-108">Du måste ha konfigurerat en träningsexperiment och prediktivt experiment som visas i [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-108">You must have set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="2f2e4-109">Prediktivt experiment måste distribueras som klassisk machine learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-109">The predictive experiment must be deployed as a Classic machine learning web service.</span></span> 
> 
> 

<span data-ttu-id="2f2e4-110">Ytterligare information om distribuera webbtjänster finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="add-a-new-endpoint"></a><span data-ttu-id="2f2e4-111">Lägg till en ny slutpunkt</span><span class="sxs-lookup"><span data-stu-id="2f2e4-111">Add a new Endpoint</span></span>
<span data-ttu-id="2f2e4-112">Den förutsägande webbtjänst som du har distribuerat innehåller en bedömningsslutpunkten som hålls synkroniserade med den ursprungliga utbildningen och bedömningen experiment tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-112">The Predictive Web Service that you deployed contains a default scoring endpoint that is kept in sync with the original training and scoring experiments trained model.</span></span> <span data-ttu-id="2f2e4-113">Om du vill uppdatera webbtjänsten till med en ny tränad modell, måste du skapa en ny bedömningsprofil slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-113">To update your web service to with a new trained model, you must create a new scoring endpoint.</span></span> 

<span data-ttu-id="2f2e4-114">Skapa en ny bedömningsprofil slutpunkt på den förutsägande webbtjänst som kan uppdateras med den tränade modellen:</span><span class="sxs-lookup"><span data-stu-id="2f2e4-114">To create a new scoring endpoint, on the Predictive Web Service that can be updated with the trained model:</span></span>

> [!NOTE]
> <span data-ttu-id="2f2e4-115">Var noga med att du lägger till slutpunkten förutsägande webbtjänsten inte utbildning-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-115">Be sure you are adding the endpoint to the Predictive Web Service, not the Training Web Service.</span></span> <span data-ttu-id="2f2e4-116">Om du har distribuerat en utbildnings- och en förutsägbar webbtjänst korrekt, bör du se två separata webbtjänster som anges.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-116">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate web services listed.</span></span> <span data-ttu-id="2f2e4-117">Förutsägande webbtjänsten ska avslutas med ”[förutsägande exp.]”.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-117">The Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

<span data-ttu-id="2f2e4-118">Det finns tre sätt som du kan lägga till en ny slutpunkt till en webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="2f2e4-118">There are three ways in which you can add a new end point to a web service:</span></span>

1. <span data-ttu-id="2f2e4-119">Programmässigt</span><span class="sxs-lookup"><span data-stu-id="2f2e4-119">Programmatically</span></span>
2. <span data-ttu-id="2f2e4-120">Webbtjänster för Microsoft Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2f2e4-120">Use the Microsoft Azure Web Services portal</span></span>
3. <span data-ttu-id="2f2e4-121">Använd den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2f2e4-121">Use the Azure classic portal</span></span>

### <a name="programmatically-add-an-endpoint"></a><span data-ttu-id="2f2e4-122">Lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="2f2e4-122">Programmatically add an endpoint</span></span>
<span data-ttu-id="2f2e4-123">Du kan lägga till bedömningsprofil slutpunkter som använder exempelkoden i den här [github-lagringsplatsen](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-123">You can add scoring endpoints using the sample code provided in this [github repository](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).</span></span>

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a><span data-ttu-id="2f2e4-124">Använda webbtjänster för Microsoft Azure-portalen för att lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="2f2e4-124">Use the Microsoft Azure Web Services portal to add an endpoint</span></span>
1. <span data-ttu-id="2f2e4-125">Klicka på webbtjänster på kolumnen navigeringen till vänster i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-125">In Machine Learning Studio, on the left navigation column, click Web Services.</span></span>
2. <span data-ttu-id="2f2e4-126">Längst ned i web service instrumentpanelen klickar du på **hantera slutpunkter preview**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-126">At the bottom of the web service dashboard, click **Manage endpoints preview**.</span></span>
3. <span data-ttu-id="2f2e4-127">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-127">Click **Add**.</span></span>
4. <span data-ttu-id="2f2e4-128">Skriv ett namn och beskrivning för den nya slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-128">Type a name and description for the new endpoint.</span></span> <span data-ttu-id="2f2e4-129">Välj loggningsnivån och om exempeldata är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-129">Select the logging level and whether sample data is enabled.</span></span> <span data-ttu-id="2f2e4-130">Mer information om loggning finns [aktivera loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-130">For more information on logging, see [Enable logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>

### <a name="use-the-azure-classic-portal-to-add-an-endpoint"></a><span data-ttu-id="2f2e4-131">Använd den klassiska Azure-portalen för att lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="2f2e4-131">Use the Azure classic portal to add an endpoint</span></span>
1. <span data-ttu-id="2f2e4-132">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-132">Sign in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="2f2e4-133">I den vänstra menyn klickar du på **Maskininlärning**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-133">In the left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="2f2e4-134">Klicka på arbetsytan under namn och klicka sedan på **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-134">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="2f2e4-135">Under namn, klickar du på **inventering modellen [förutsägande exp].** .</span><span class="sxs-lookup"><span data-stu-id="2f2e4-135">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="2f2e4-136">Längst ned på sidan klickar du på **Lägg till slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-136">At the bottom of the page, click **Add Endpoint**.</span></span> <span data-ttu-id="2f2e4-137">Mer information om att lägga till slutpunkter finns [skapar slutpunkter](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-137">For more information on adding endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span> 

## <a name="update-the-added-endpoints-trained-model"></a><span data-ttu-id="2f2e4-138">Uppdatera tillagda slutpunkten Trained Model</span><span class="sxs-lookup"><span data-stu-id="2f2e4-138">Update the added endpoint’s Trained Model</span></span>
<span data-ttu-id="2f2e4-139">Om du vill slutföra omtränings, måste du uppdatera den tränade modellen för den nya slutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-139">To complete the retraining process, you must update the trained model of the new endpoint that you added.</span></span>

* <span data-ttu-id="2f2e4-140">Om du har lagt till nya slutpunkten med den klassiska Azure-portalen, du kan klicka på namnet på den nya slutpunkten i portalen kommer **UpdateResource** länken för att hämta Webbadress måste du uppdatera slutpunkten modellen.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-140">If you added the new endpoint using the classic Azure portal, you can click the new endpoint's name in the portal, then the **UpdateResource** link to get the URL you would need to update the endpoint's model.</span></span>
* <span data-ttu-id="2f2e4-141">Om du har lagt till slutpunkten med exempelkoden inkluderar plats för hjälp-URL som identifieras av den *HelpLocationURL* värdet i utdata.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-141">If you added the endpoint using the sample code, this includes location of the help URL identified by the *HelpLocationURL* value in the output.</span></span>

<span data-ttu-id="2f2e4-142">Att hämta sökvägen-URL:</span><span class="sxs-lookup"><span data-stu-id="2f2e4-142">To retrieve the path URL:</span></span>

1. <span data-ttu-id="2f2e4-143">Kopiera och klistra in Webbadressen i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-143">Copy and paste the URL into your browser.</span></span>
2. <span data-ttu-id="2f2e4-144">Klicka på länken Update resurs.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-144">Click the Update Resource link.</span></span>
3. <span data-ttu-id="2f2e4-145">Kopiera URL-Adressen för POST till PATCH-begäran.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-145">Copy the POST URL of the PATCH request.</span></span> <span data-ttu-id="2f2e4-146">Exempel:</span><span class="sxs-lookup"><span data-stu-id="2f2e4-146">For example:</span></span>
   
     <span data-ttu-id="2f2e4-147">KORRIGERING AV URL: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2</span><span class="sxs-lookup"><span data-stu-id="2f2e4-147">PATCH URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2</span></span>

<span data-ttu-id="2f2e4-148">Du kan nu använda den tränade modellen för att uppdatera bedömningsprofil slutpunkten som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-148">You can now use the trained model to update the scoring endpoint that you created previously.</span></span>

<span data-ttu-id="2f2e4-149">Följande exempelkod visar hur du använder den *BaseLocation*, *RelativeLocation*, *SasBlobToken*, och KORRIGERA URL för att uppdatera slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-149">The following sample code shows you how to use the *BaseLocation*, *RelativeLocation*, *SasBlobToken*, and PATCH URL to update the endpoint.</span></span>

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
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
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

<span data-ttu-id="2f2e4-150">Den *apiKey* och *endpointUrl* för anropet kan hämtas från slutpunkten instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-150">The *apiKey* and the *endpointUrl* for the call can be obtained from endpoint dashboard.</span></span>

<span data-ttu-id="2f2e4-151">Värdet för den *namn* parameter i *resurser* ska matcha namnet på den sparade tränade modellen i prediktivt experiment resurs.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-151">The value of the *Name* parameter in *Resources* should match the Resource Name of the saved Trained Model in the predictive experiment.</span></span> <span data-ttu-id="2f2e4-152">Hämta resursnamnet:</span><span class="sxs-lookup"><span data-stu-id="2f2e4-152">To get the Resource Name:</span></span>

1. <span data-ttu-id="2f2e4-153">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="2f2e4-153">Sign in to the [classic Azure portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="2f2e4-154">I den vänstra menyn klickar du på **Maskininlärning**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-154">In the left menu, click **Machine Learning**.</span></span>
3. <span data-ttu-id="2f2e4-155">Klicka på arbetsytan under namn och klicka sedan på **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-155">Under Name, click your workspace and then click **Web Services**.</span></span>
4. <span data-ttu-id="2f2e4-156">Under namn, klickar du på **inventering modellen [förutsägande exp].** .</span><span class="sxs-lookup"><span data-stu-id="2f2e4-156">Under Name, click **Census Model [predictive exp.]**.</span></span>
5. <span data-ttu-id="2f2e4-157">Klicka på ny slutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-157">Click the new endpoint you added.</span></span>
6. <span data-ttu-id="2f2e4-158">Klicka på infopanelen endpoint **uppdatering resurs**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-158">On the endpoint dashboard, click **Update Resource**.</span></span>
7. <span data-ttu-id="2f2e4-159">På sidan uppdatera resurs API-dokumentationen för webbtjänsten hittar du den **resursnamnet** under **uppdateras resurser**.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-159">On the Update Resource API Documentation page for the web service, you can find the **Resource Name** under **Updatable Resources**.</span></span>

<span data-ttu-id="2f2e4-160">Om din SAS-token upphör att gälla innan du har uppdaterat slutpunkten, måste du utföra GET med jobb-Id för att få en ny token.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-160">If your SAS token expires before you finish updating the endpoint, you must perform a GET with the Job Id to obtain a fresh token.</span></span>

<span data-ttu-id="2f2e4-161">När koden har körts, ska ny slutpunkt börja använda retrained modellen cirka 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-161">When the code has successfully run, the new endpoint should start using the retrained model in approximately 30 seconds.</span></span>

## <a name="summary"></a><span data-ttu-id="2f2e4-162">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="2f2e4-162">Summary</span></span>
<span data-ttu-id="2f2e4-163">Du kan använda Omtränings-API för att uppdatera den tränade modellen av en förutsägbar webbtjänsten för att aktivera scenarier som:</span><span class="sxs-lookup"><span data-stu-id="2f2e4-163">Using the Retraining APIs, you can update the trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="2f2e4-164">Periodiska modell via programmering med nya data.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-164">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="2f2e4-165">Distribution av en modell för kunder med målet att låta dem träna om modellen med sina egna data.</span><span class="sxs-lookup"><span data-stu-id="2f2e4-165">Distribution of a model to customers with the goal of letting them retrain the model using their own data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2f2e4-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2f2e4-166">Next steps</span></span>
[<span data-ttu-id="2f2e4-167">Felsökning av omtränings av en klassisk Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="2f2e4-167">Troubleshooting the retraining of an Azure Machine Learning classic web service</span></span>](machine-learning-troubleshooting-retraining-models.md)

