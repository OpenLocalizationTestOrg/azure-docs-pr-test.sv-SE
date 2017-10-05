---
title: "(föråldrad) Publicera maskininlärning webbtjänsten Azure Marketplace | Microsoft Docs"
description: "(föråldrad) Så här publicerar du din Azure Machine Learning-webbtjänst på Azure Marketplace"
services: machine-learning
documentationcenter: 
author: BharathS
manager: jhubbard
editor: cgronlun
ms.assetid: 68e908be-3a99-4cd7-9517-e2b5f2f341b8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: machine-learning-gallery-experiments
redirect_document_id: TRUE
ms.openlocfilehash: 3e3420872f0c604e027d1f309a6de6f52a5a788c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-to-the-azure-marketplace"></a><span data-ttu-id="8c9ae-103">(föråldrad) Publicera Azure Machine Learning-webbtjänst på Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="8c9ae-103">(deprecated) Publish Azure Machine Learning Web Service to the Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="8c9ae-104">Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="8c9ae-105">Därför kan är den här artikeln inaktuell.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="8c9ae-106">Alternativt kan du publicera Machine Learning-experiment till den [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) för datavetenskap community.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-106">As an alternative, you can publish your Machine Learning experiments to the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for the benefit of the data science community.</span></span> <span data-ttu-id="8c9ae-107">Mer information finns i [resursen och identifiera resurser i Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="8c9ae-107">For more information, see [Share and discover resources in the Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>

<span data-ttu-id="8c9ae-108">Azure Marketplace ger möjlighet att publicera Azure Machine Learning-webbtjänster som betald eller kostnadsfri tjänster för användning med externa kunder.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-108">The Azure Marketplace provides the ability to publish Azure Machine Learning web services as paid or free services for consumption by external customers.</span></span> <span data-ttu-id="8c9ae-109">Den här artikeln innehåller en översikt över processen med länkar till riktlinjer för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-109">This article provides an overview of that process with links to guidelines to get you started.</span></span> <span data-ttu-id="8c9ae-110">Med den här processen kan du webbtjänster tillgänglig för andra utvecklare att använda i sina program.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-110">By using this process, you can make your web services available for other developers to consume in their applications.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-the-publishing-process"></a><span data-ttu-id="8c9ae-111">Översikt över publiceringsprocessen</span><span class="sxs-lookup"><span data-stu-id="8c9ae-111">Overview of the publishing process</span></span>
<span data-ttu-id="8c9ae-112">Här följer stegen för att publicera en Azure Machine Learning-webbtjänst på Azure Marketplace:</span><span class="sxs-lookup"><span data-stu-id="8c9ae-112">The following are the steps for publishing an Azure Machine Learning web service to Azure Marketplace:</span></span>

1. <span data-ttu-id="8c9ae-113">Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar</span><span class="sxs-lookup"><span data-stu-id="8c9ae-113">Create and publish a Machine Learning Request-Response service (RRS)</span></span>
2. <span data-ttu-id="8c9ae-114">Distribuera tjänsten till produktion och hämta information för API-nyckel och OData-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-114">Deploy the service to production, and obtain the API Key and OData endpoint information.</span></span>
3. <span data-ttu-id="8c9ae-115">Använda URL: en för publicerade webbtjänsten för att publicera [Azure Marketplace (Data marknaden)](https://publish.windowsazure.com/workspace/)</span><span class="sxs-lookup"><span data-stu-id="8c9ae-115">Use the URL of the published web service to publish to [Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/)</span></span> 
4. <span data-ttu-id="8c9ae-116">När fram erbjudandet granskat och måste godkännas innan dina kunder kan starta köpa den.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-116">Once submitted, your offer is reviewed and needs to be approved before your customers can start purchasing it.</span></span> <span data-ttu-id="8c9ae-117">Publiceringsprocessen kan ta några arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-117">The publishing process can take a few business days.</span></span> 

## <a name="walk-through"></a><span data-ttu-id="8c9ae-118">Genomgång</span><span class="sxs-lookup"><span data-stu-id="8c9ae-118">Walk through</span></span>
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a><span data-ttu-id="8c9ae-119">Steg 1: Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar</span><span class="sxs-lookup"><span data-stu-id="8c9ae-119">Step 1: Create and publish a Machine Learning Request-Response service (RRS)</span></span>
 <span data-ttu-id="8c9ae-120">Om du redan inte har gjort det, ta en titt på den här [igenom](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8c9ae-120">If you have not done this already, please take a look at this [walk through](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

### <a name="step-2-deploy-the-service-to-production-and-obtain-the-api-key-and-odata-endpoint-information"></a><span data-ttu-id="8c9ae-121">Steg 2: Distribuera tjänsten till produktion och hämta API-nyckel och information för OData-slutpunkt</span><span class="sxs-lookup"><span data-stu-id="8c9ae-121">Step 2: Deploy the service to production, and obtain the API Key and OData endpoint information</span></span>
1. <span data-ttu-id="8c9ae-122">Från den [klassiska Azure-portalen](http://manage.windowsazure.com), Välj den **MACHINE LEARNING** från det vänstra navigeringsfältet och välj din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-122">From the [Azure Classic Portal](http://manage.windowsazure.com), select the **MACHINE LEARNING** option from the left navigation bar, and select your workspace.</span></span> 
2. <span data-ttu-id="8c9ae-123">Klicka på den **WEB SERVICES** fliken och markera den webbtjänst som du vill publicera på marketplace.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-123">Click on the **WEB SERVICES** tab, and select the web service you would like to publish to the marketplace.</span></span>
   
    ![Azure Marketplace][workspace]
3. <span data-ttu-id="8c9ae-125">Välj den slutpunkt som du vill ha marketplace använda.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-125">Select the endpoint you would like to have the marketplace consume.</span></span> <span data-ttu-id="8c9ae-126">Om du inte har skapat några ytterligare slutpunkter kan du välja den **standard** slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-126">If you have not created any additional endpoints, you can select the **Default** endpoint.</span></span>
4. <span data-ttu-id="8c9ae-127">När du har klickat på slutpunkten, kommer du att kunna se den **API-NYCKELN**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-127">Once you have clicked on the endpoint, you will be able to see the **API KEY**.</span></span> <span data-ttu-id="8c9ae-128">Du behöver denna typ av information vid ett senare tillfälle i steg3, så gör en kopia av den.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-128">You will need this piece of information later on in Step 3, so make a copy of it.</span></span>
   
    ![Azure Marketplace][apikey]
5. <span data-ttu-id="8c9ae-130">Klicka på den **frågor och svar** metoden nu inte stöder vi batch execution publiceringstjänster på Marketplace.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-130">Click on the **REQUEST/RESPONSE** method, at this point we do not support publishing batch execution services to the marketplace.</span></span> <span data-ttu-id="8c9ae-131">Som tar dig vidare till sidan API hjälp för metoden begäran och svar.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-131">That will take you to the API help page for the Request/Response method.</span></span>
6. <span data-ttu-id="8c9ae-132">Kopiera den **OData slutpunktsadress**, måste den här informationen senare i steg3.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-132">Copy the **OData Endpoint Address**, you will need this information later on in Step 3.</span></span>
   
    ![Azure Marketplace][odata]

<span data-ttu-id="8c9ae-134">distribuera tjänsten till produktion.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-134">deploy the service into production.</span></span>

### <a name="step-3-use-the-url-of-the-published-web-service-to-publish-to-azure-marketplace-datamarket"></a><span data-ttu-id="8c9ae-135">Steg 3: Använda Webbadressen för den publicerade webbtjänsten för att publicera Azure Marketplace (DataMarket)</span><span class="sxs-lookup"><span data-stu-id="8c9ae-135">Step 3: Use the URL of the published web service to publish to Azure Marketplace (DataMarket)</span></span>
1. <span data-ttu-id="8c9ae-136">Navigera till [Azure Marketplace (Data marknaden)](http://datamarket.azure.com/home)</span><span class="sxs-lookup"><span data-stu-id="8c9ae-136">Navigate to [Azure Marketplace (Data Market)](http://datamarket.azure.com/home)</span></span> 
2. <span data-ttu-id="8c9ae-137">Klicka på den **publicera** längst upp på sidan.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-137">Click on the **Publish** link at the top of the page.</span></span> <span data-ttu-id="8c9ae-138">Detta tar du det [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="8c9ae-138">This will take you to the [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span></span>
3. <span data-ttu-id="8c9ae-139">Klicka på den **utgivare** avsnittet om du vill registrera som en utgivare.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-139">Click on the **publishers** section to register as a publisher.</span></span>
4. <span data-ttu-id="8c9ae-140">När du skapar ett nytt erbjudande väljer **datatjänster**, klicka på **skapa en ny datatjänst**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-140">When creating a new offer, select **Data Services**, then click **Create a New Data Service**.</span></span> 
   
   ![Azure Marketplace][image1]
   
   <br />
5. <span data-ttu-id="8c9ae-142">Under **planer** innehåller information om dina erbjudanden, inklusive en prisavtal.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-142">Under **Plans** provide information on your offering, including a pricing plan.</span></span> <span data-ttu-id="8c9ae-143">Bestäm om du kommer att erbjuda en kostnadsfria eller betalda tjänst.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-143">Decide if you will offer a free or paid service.</span></span> <span data-ttu-id="8c9ae-144">Om du vill hämta betald lämna betalningsinformation, till exempel din bank och skatterna information.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-144">To get paid, provide payment information such as your bank and tax information.</span></span>
6. <span data-ttu-id="8c9ae-145">Under **marknadsföring** innehåller information om erbjudandet, till exempel namnet och beskrivningen för ditt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-145">Under **Marketing** provide information about your offer, such as the title and description for your offer.</span></span>
7. <span data-ttu-id="8c9ae-146">Under **priser** du kan ange priset för din planer för specifika länder eller låta systemet ”autoprice” erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-146">Under **Pricing** you can set the price for your plans for specific countries, or let the system "autoprice" your offer.</span></span>
8. <span data-ttu-id="8c9ae-147">På den **datatjänsten** klickar du på **webbtjänsten** som den **datakällan**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-147">On the **Data Service** tab, click **Web Service** as the **Data Source**.</span></span>
   
    ![Azure Marketplace][image2]
9. <span data-ttu-id="8c9ae-149">Hämta den web service URL: en och API-nyckeln från den klassiska Azure-portalen, enligt beskrivningen i steg 2 ovan.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-149">Get the web service URL and API key from the Azure Classic Portal, as explained in step 2 above.</span></span>
10. <span data-ttu-id="8c9ae-150">I dialogrutan Marketplace datatjänst inställningar klistrar du in adress för OData-slutpunkt i den **Tjänstwebbadress** textruta.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-150">In the Marketplace Data Service setup dialog box, paste the OData endpoint address into the **Service URL** text box.</span></span>
11. <span data-ttu-id="8c9ae-151">För **autentisering**, Välj **huvud** som den **autentiseringsschema**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-151">For **Authentication**, choose **Header** as the **Authentication Scheme**.</span></span>
    
    * <span data-ttu-id="8c9ae-152">Ange ”tillstånd” för den **huvudnamnet**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-152">Enter "Authorization" for the **Header Name**.</span></span>
    * <span data-ttu-id="8c9ae-153">För den **Rubrikvärdet**, Skriv ”ägar” (utan citattecken), klicka på den **utrymme** menyraden och sedan klistra in API-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-153">For the **Header Value**, enter "Bearer" (without the quotation marks), click the **Space** bar, and then paste the API key.</span></span>
    * <span data-ttu-id="8c9ae-154">Välj den **den här tjänsten är OData** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-154">Select the **This Service is OData** check box.</span></span>
    * <span data-ttu-id="8c9ae-155">Klicka på **Testa anslutning** att testa anslutningen.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-155">Click **Test Connection** to test the connection.</span></span>
12. <span data-ttu-id="8c9ae-156">Under **kategorier**, se till att **Maskininlärning** är markerad.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-156">Under **Categories**, ensure **Machine Learning** is selected.</span></span>
13. <span data-ttu-id="8c9ae-157">När du är klar att ange alla metadata om erbjudandet, klicka på **publicera**, och sedan **Push till Förproduktion**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-157">When you are done entering all the metadata about your offer, click on **Publish**, and then **Push to Staging**.</span></span> <span data-ttu-id="8c9ae-158">Nu meddelas du om eventuella återstående problem som du behöver åtgärda.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-158">At this point, you will be notified of any remaining issues that you need to fix.</span></span>
14. <span data-ttu-id="8c9ae-159">När du har kontrollerat alla återstående frågor har slutförts klickar du på **begära tillstånd att skicka till produktion**.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-159">After you have ensured completion of all the outstanding issues, click on **Request approval to push to Production**.</span></span> <span data-ttu-id="8c9ae-160">Publiceringsprocessen kan ta några arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="8c9ae-160">The publishing process can take a few business days.</span></span> 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

