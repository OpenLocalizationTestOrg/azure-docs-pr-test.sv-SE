---
title: "aaa(deprecated) publicera maskininlärning web service tooAzure Marketplace | Microsoft Docs"
description: "(föråldrad) Hur toopublish din Azure Machine Learning-webbtjänst toohello Azure Marketplace"
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
redirect_document_id: True
ms.openlocfilehash: 149abc3df9b79c1b37d233d5e85e803592ff1020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-toohello-azure-marketplace"></a><span data-ttu-id="30e6d-103">(föråldrad) Publicera Azure Machine Learning-webbtjänst toohello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="30e6d-103">(deprecated) Publish Azure Machine Learning Web Service toohello Azure Marketplace</span></span>

> [!NOTE]
> <span data-ttu-id="30e6d-104">Tjänsterna och DataMarket är som har återkallats och befintliga prenumerationer ska tas bort och avbrutits startar 3/31/2017.</span><span class="sxs-lookup"><span data-stu-id="30e6d-104">DataMarket and Data Services are being retired, and existing subscriptions will be retired and cancelled starting 3/31/2017.</span></span> <span data-ttu-id="30e6d-105">Därför kan är den här artikeln inaktuell.</span><span class="sxs-lookup"><span data-stu-id="30e6d-105">As a result, this article is being deprecated.</span></span> 
> 
> <span data-ttu-id="30e6d-106">Alternativt kan du publicera din Machine Learning-experiment toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello förmån för hello datavetenskap community.</span><span class="sxs-lookup"><span data-stu-id="30e6d-106">As an alternative, you can publish your Machine Learning experiments toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) for hello benefit of hello data science community.</span></span> <span data-ttu-id="30e6d-107">Mer information finns i [resursen och identifiera resurser i hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span><span class="sxs-lookup"><span data-stu-id="30e6d-107">For more information, see [Share and discover resources in hello Cortana Intelligence Gallery](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).</span></span>

<span data-ttu-id="30e6d-108">hello Azure Marketplace innehåller hello möjlighet toopublish Azure Machine Learning-webbtjänster som betald eller frigör tjänster för användning med externa kunder.</span><span class="sxs-lookup"><span data-stu-id="30e6d-108">hello Azure Marketplace provides hello ability toopublish Azure Machine Learning web services as paid or free services for consumption by external customers.</span></span> <span data-ttu-id="30e6d-109">Den här artikeln innehåller en översikt över processen med länkar tooguidelines tooget du startade.</span><span class="sxs-lookup"><span data-stu-id="30e6d-109">This article provides an overview of that process with links tooguidelines tooget you started.</span></span> <span data-ttu-id="30e6d-110">Med den här processen kan du webbtjänster tillgänglig för andra utvecklare tooconsume i sina program.</span><span class="sxs-lookup"><span data-stu-id="30e6d-110">By using this process, you can make your web services available for other developers tooconsume in their applications.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-hello-publishing-process"></a><span data-ttu-id="30e6d-111">Översikt över hello publiceringsprocessen</span><span class="sxs-lookup"><span data-stu-id="30e6d-111">Overview of hello publishing process</span></span>
<span data-ttu-id="30e6d-112">hello följande är hello steg för att publicera en Azure Machine Learning web service tooAzure Marketplace:</span><span class="sxs-lookup"><span data-stu-id="30e6d-112">hello following are hello steps for publishing an Azure Machine Learning web service tooAzure Marketplace:</span></span>

1. <span data-ttu-id="30e6d-113">Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar</span><span class="sxs-lookup"><span data-stu-id="30e6d-113">Create and publish a Machine Learning Request-Response service (RRS)</span></span>
2. <span data-ttu-id="30e6d-114">Distribuera hello service tooproduction och hämta hello information om API-nyckel och OData-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="30e6d-114">Deploy hello service tooproduction, and obtain hello API Key and OData endpoint information.</span></span>
3. <span data-ttu-id="30e6d-115">Använd hello URL för hello publicerade web service toopublish för[Azure Marketplace (Data marknaden)](https://publish.windowsazure.com/workspace/)</span><span class="sxs-lookup"><span data-stu-id="30e6d-115">Use hello URL of hello published web service toopublish too[Azure Marketplace (Data Market)](https://publish.windowsazure.com/workspace/)</span></span> 
4. <span data-ttu-id="30e6d-116">När fram erbjudandet granskas och måste toobe godkänts före kunderna kan börja köpa den.</span><span class="sxs-lookup"><span data-stu-id="30e6d-116">Once submitted, your offer is reviewed and needs toobe approved before your customers can start purchasing it.</span></span> <span data-ttu-id="30e6d-117">hello publishing processen kan ta några arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="30e6d-117">hello publishing process can take a few business days.</span></span> 

## <a name="walk-through"></a><span data-ttu-id="30e6d-118">Genomgång</span><span class="sxs-lookup"><span data-stu-id="30e6d-118">Walk through</span></span>
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a><span data-ttu-id="30e6d-119">Steg 1: Skapa och publicera en tjänst (RR) för Machine Learning begäran och svar</span><span class="sxs-lookup"><span data-stu-id="30e6d-119">Step 1: Create and publish a Machine Learning Request-Response service (RRS)</span></span>
 <span data-ttu-id="30e6d-120">Om du redan inte har gjort det, ta en titt på den här [igenom](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="30e6d-120">If you have not done this already, please take a look at this [walk through](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

### <a name="step-2-deploy-hello-service-tooproduction-and-obtain-hello-api-key-and-odata-endpoint-information"></a><span data-ttu-id="30e6d-121">Steg 2: Distribuera hello service tooproduction och hämta hello information om API-nyckel och OData-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="30e6d-121">Step 2: Deploy hello service tooproduction, and obtain hello API Key and OData endpoint information</span></span>
1. <span data-ttu-id="30e6d-122">Från hello [klassiska Azure-portalen](http://manage.windowsazure.com)väljer hello **MASKININLÄRNING** från hello vänstra navigeringsfältet och välj din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="30e6d-122">From hello [Azure Classic Portal](http://manage.windowsazure.com), select hello **MACHINE LEARNING** option from hello left navigation bar, and select your workspace.</span></span> 
2. <span data-ttu-id="30e6d-123">Klicka på hello **WEB SERVICES** fliken och väljer hello-webbtjänst som du vill att toopublish toohello marketplace.</span><span class="sxs-lookup"><span data-stu-id="30e6d-123">Click on hello **WEB SERVICES** tab, and select hello web service you would like toopublish toohello marketplace.</span></span>
   
    ![Azure Marketplace][workspace]
3. <span data-ttu-id="30e6d-125">Välj hello-slutpunkt som du skulle t.ex toohave hello marketplace använder.</span><span class="sxs-lookup"><span data-stu-id="30e6d-125">Select hello endpoint you would like toohave hello marketplace consume.</span></span> <span data-ttu-id="30e6d-126">Om du inte har skapat några ytterligare slutpunkter kan du välja hello **standard** slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="30e6d-126">If you have not created any additional endpoints, you can select hello **Default** endpoint.</span></span>
4. <span data-ttu-id="30e6d-127">När du har klickat på hello slutpunkt, kommer du att kunna toosee hello **API-NYCKELN**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-127">Once you have clicked on hello endpoint, you will be able toosee hello **API KEY**.</span></span> <span data-ttu-id="30e6d-128">Du behöver denna typ av information vid ett senare tillfälle i steg3, så gör en kopia av den.</span><span class="sxs-lookup"><span data-stu-id="30e6d-128">You will need this piece of information later on in Step 3, so make a copy of it.</span></span>
   
    ![Azure Marketplace][apikey]
5. <span data-ttu-id="30e6d-130">Klicka på hello **frågor och svar** services toohello marketplace-metoden i det här stadiet publishing batchkörning inte stöds.</span><span class="sxs-lookup"><span data-stu-id="30e6d-130">Click on hello **REQUEST/RESPONSE** method, at this point we do not support publishing batch execution services toohello marketplace.</span></span> <span data-ttu-id="30e6d-131">Som tar dig toohello API hjälpsidan för hello begäranden/svar-metoden.</span><span class="sxs-lookup"><span data-stu-id="30e6d-131">That will take you toohello API help page for hello Request/Response method.</span></span>
6. <span data-ttu-id="30e6d-132">Kopiera hello **OData slutpunktsadress**, måste den här informationen senare i steg3.</span><span class="sxs-lookup"><span data-stu-id="30e6d-132">Copy hello **OData Endpoint Address**, you will need this information later on in Step 3.</span></span>
   
    ![Azure Marketplace][odata]

<span data-ttu-id="30e6d-134">distribuera hello tjänsten till produktion.</span><span class="sxs-lookup"><span data-stu-id="30e6d-134">deploy hello service into production.</span></span>

### <a name="step-3-use-hello-url-of-hello-published-web-service-toopublish-tooazure-marketplace-datamarket"></a><span data-ttu-id="30e6d-135">Steg 3: Använd hello URL för hello publicerade web service toopublish tooAzure Marketplace (DataMarket)</span><span class="sxs-lookup"><span data-stu-id="30e6d-135">Step 3: Use hello URL of hello published web service toopublish tooAzure Marketplace (DataMarket)</span></span>
1. <span data-ttu-id="30e6d-136">Navigera för[Azure Marketplace (Data marknaden)](http://datamarket.azure.com/home)</span><span class="sxs-lookup"><span data-stu-id="30e6d-136">Navigate too[Azure Marketplace (Data Market)](http://datamarket.azure.com/home)</span></span> 
2. <span data-ttu-id="30e6d-137">Klicka på hello **publicera** länk hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="30e6d-137">Click on hello **Publish** link at hello top of hello page.</span></span> <span data-ttu-id="30e6d-138">Detta tar du toohello [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span><span class="sxs-lookup"><span data-stu-id="30e6d-138">This will take you toohello [Microsoft Azure Publishing Portal](https://publish.windowsazure.com)</span></span>
3. <span data-ttu-id="30e6d-139">Klicka på hello **utgivare** avsnittet tooregister som utgivare.</span><span class="sxs-lookup"><span data-stu-id="30e6d-139">Click on hello **publishers** section tooregister as a publisher.</span></span>
4. <span data-ttu-id="30e6d-140">När du skapar ett nytt erbjudande väljer **datatjänster**, klicka på **skapa en ny datatjänst**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-140">When creating a new offer, select **Data Services**, then click **Create a New Data Service**.</span></span> 
   
   ![Azure Marketplace][image1]
   
   <br />
5. <span data-ttu-id="30e6d-142">Under **planer** innehåller information om dina erbjudanden, inklusive en prisavtal.</span><span class="sxs-lookup"><span data-stu-id="30e6d-142">Under **Plans** provide information on your offering, including a pricing plan.</span></span> <span data-ttu-id="30e6d-143">Bestäm om du kommer att erbjuda en kostnadsfria eller betalda tjänst.</span><span class="sxs-lookup"><span data-stu-id="30e6d-143">Decide if you will offer a free or paid service.</span></span> <span data-ttu-id="30e6d-144">tooget betald, lämna betalningsinformation, till exempel din bank och skatterna information.</span><span class="sxs-lookup"><span data-stu-id="30e6d-144">tooget paid, provide payment information such as your bank and tax information.</span></span>
6. <span data-ttu-id="30e6d-145">Under **marknadsföring** innehåller information om erbjudandet, till exempel hello rubrik och beskrivning för ditt erbjudande.</span><span class="sxs-lookup"><span data-stu-id="30e6d-145">Under **Marketing** provide information about your offer, such as hello title and description for your offer.</span></span>
7. <span data-ttu-id="30e6d-146">Under **priser** du kan ange hello priset för din planer för specifika länder eller låta hello system ”autoprice” erbjudandet.</span><span class="sxs-lookup"><span data-stu-id="30e6d-146">Under **Pricing** you can set hello price for your plans for specific countries, or let hello system "autoprice" your offer.</span></span>
8. <span data-ttu-id="30e6d-147">På hello **datatjänsten** klickar du på **webbtjänsten** som hello **datakällan**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-147">On hello **Data Service** tab, click **Web Service** as hello **Data Source**.</span></span>
   
    ![Azure Marketplace][image2]
9. <span data-ttu-id="30e6d-149">Hämta hello web service URL: en och API-nyckeln från hello klassiska Azure-portalen, enligt beskrivningen i steg 2 ovan.</span><span class="sxs-lookup"><span data-stu-id="30e6d-149">Get hello web service URL and API key from hello Azure Classic Portal, as explained in step 2 above.</span></span>
10. <span data-ttu-id="30e6d-150">Klistra in hello OData slutpunktsadress i hello hello Marketplace datatjänst inställningar i dialogrutan **Tjänstwebbadress** textruta.</span><span class="sxs-lookup"><span data-stu-id="30e6d-150">In hello Marketplace Data Service setup dialog box, paste hello OData endpoint address into hello **Service URL** text box.</span></span>
11. <span data-ttu-id="30e6d-151">För **autentisering**, Välj **huvud** som hello **autentiseringsschema**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-151">For **Authentication**, choose **Header** as hello **Authentication Scheme**.</span></span>
    
    * <span data-ttu-id="30e6d-152">Ange ”tillstånd” för hello **huvudnamnet**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-152">Enter "Authorization" for hello **Header Name**.</span></span>
    * <span data-ttu-id="30e6d-153">För hello **Rubrikvärdet**, Skriv ”ägar” (utan citattecken hello), klicka på hello **utrymme** menyraden och sedan klistra in hello API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="30e6d-153">For hello **Header Value**, enter "Bearer" (without hello quotation marks), click hello **Space** bar, and then paste hello API key.</span></span>
    * <span data-ttu-id="30e6d-154">Välj hello **den här tjänsten är OData** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="30e6d-154">Select hello **This Service is OData** check box.</span></span>
    * <span data-ttu-id="30e6d-155">Klicka på **Testanslutningen** tootest hello anslutning.</span><span class="sxs-lookup"><span data-stu-id="30e6d-155">Click **Test Connection** tootest hello connection.</span></span>
12. <span data-ttu-id="30e6d-156">Under **kategorier**, se till att **Maskininlärning** är markerad.</span><span class="sxs-lookup"><span data-stu-id="30e6d-156">Under **Categories**, ensure **Machine Learning** is selected.</span></span>
13. <span data-ttu-id="30e6d-157">När du är klar att ange alla hello metadata om erbjudandet, klickar du på **publicera**, och sedan **Push tooStaging**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-157">When you are done entering all hello metadata about your offer, click on **Publish**, and then **Push tooStaging**.</span></span> <span data-ttu-id="30e6d-158">Nu är meddelas du om eventuella återstående problem som du behöver toofix.</span><span class="sxs-lookup"><span data-stu-id="30e6d-158">At this point, you will be notified of any remaining issues that you need toofix.</span></span>
14. <span data-ttu-id="30e6d-159">När du har kontrollerat alla hello utestående problem har slutförts klickar du på **begära godkännande toopush tooProduction**.</span><span class="sxs-lookup"><span data-stu-id="30e6d-159">After you have ensured completion of all hello outstanding issues, click on **Request approval toopush tooProduction**.</span></span> <span data-ttu-id="30e6d-160">hello publishing processen kan ta några arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="30e6d-160">hello publishing process can take a few business days.</span></span> 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

