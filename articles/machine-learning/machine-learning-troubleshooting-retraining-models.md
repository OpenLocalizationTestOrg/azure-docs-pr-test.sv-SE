---
title: "Felsöka omtränings en Azure Machine Learning klassiska webbtjänst | Microsoft Docs"
description: "Identifiera och lösa vanliga problem uppstod när du omtränings modellen för en Azure Machine Learning-webbtjänst."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: fc36499ebff88c86635228ff899c85e9166aabed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a><span data-ttu-id="7ab8e-103">Felsökning av omtränings av en Azure Machine Learning klassiska-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="7ab8e-103">Troubleshooting the retraining of an Azure Machine Learning Classic Web service</span></span>
## <a name="retraining-overview"></a><span data-ttu-id="7ab8e-104">Omtränings översikt</span><span class="sxs-lookup"><span data-stu-id="7ab8e-104">Retraining overview</span></span>
<span data-ttu-id="7ab8e-105">När du distribuerar en prediktivt experiment som en bedömningsprofil webbtjänst är en statisk modell.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-105">When you deploy a predictive experiment as a scoring web service it is a static model.</span></span> <span data-ttu-id="7ab8e-106">När nya data blir tillgängliga eller när konsumenten API har sina egna data, måste vara retrained modellen.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-106">As new data becomes available or when the consumer of the API has their own data, the model needs to be retrained.</span></span> 

<span data-ttu-id="7ab8e-107">En fullständig genomgång av hur omtränings klassiska webbtjänsten finns [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="7ab8e-107">For a complete walkthrough of the retraining process of a Classic Web service, see [Retrain Machine Learning Models Programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

## <a name="retraining-process"></a><span data-ttu-id="7ab8e-108">Omtränings process</span><span class="sxs-lookup"><span data-stu-id="7ab8e-108">Retraining process</span></span>
<span data-ttu-id="7ab8e-109">När du behöver träna om webbtjänsten måste du lägga till vissa ytterligare delar:</span><span class="sxs-lookup"><span data-stu-id="7ab8e-109">When you need to retrain the Web service, you must add some additional pieces:</span></span>

* <span data-ttu-id="7ab8e-110">En webbtjänst som distribueras från utbildning experimentet.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-110">A Web service deployed from the Training Experiment.</span></span> <span data-ttu-id="7ab8e-111">Experimentet måste ha en **Web Service utdata** modulen ansluten till utdataporten för den **Träningsmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-111">The experiment must have a **Web Service Output** module attached to the output of the **Train Model** module.</span></span>  
  
    ![Koppla web service utdata till train-modell.][image1]
* <span data-ttu-id="7ab8e-113">En ny slutpunkt som lagts till bedömningsprofil webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-113">A new endpoint added to your scoring Web service.</span></span>  <span data-ttu-id="7ab8e-114">Du kan lägga till slutpunkten programmässigt med exempelkoden som refereras i träna om Machine Learning-modeller via programmering avsnittet eller via den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-114">You can add the endpoint programmatically using the sample code referenced in the Retrain Machine Learning models programmatically topic or through the Azure classic portal.</span></span>

<span data-ttu-id="7ab8e-115">Du kan sedan använda C# exempelkoden från hjälpsidan för utbildning-webbtjänsten API för att träna om modellen.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-115">You can then use the sample C# code from the Training Web Service's API help page to retrain model.</span></span> <span data-ttu-id="7ab8e-116">När du har utvärderat resultaten och är nöjd med dem kan uppdatera du den tränade modellen bedömningen webbtjänst med hjälp av den nya slutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-116">Once you have evaluated the results and are satisfied with them, you update the trained model scoring web service using the new endpoint that you added.</span></span>

<span data-ttu-id="7ab8e-117">Med alla delar på plats är de viktigaste stegen som du måste vidta för att träna om modellen följande:</span><span class="sxs-lookup"><span data-stu-id="7ab8e-117">With all the pieces in place, the major steps you must take to retrain the model are as follows:</span></span>

1. <span data-ttu-id="7ab8e-118">Anropa webbtjänsten utbildning: anropet är att den Batch körning Service BES-, inte de begär svar Service (RR).</span><span class="sxs-lookup"><span data-stu-id="7ab8e-118">Call the Training Web Service:  The call is to the Batch Execution Service (BES), not the Request Response Service (RRS).</span></span> <span data-ttu-id="7ab8e-119">Du kan använda C# exempelkoden på hjälpsidan API för att ringa.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-119">You can use the sample C# code on the API help page to make the call.</span></span> 
2. <span data-ttu-id="7ab8e-120">Värden för att hitta den *BaseLocation*, *RelativeLocation*, och *SasBlobToken*: dessa värden returneras i utdata från anrop till webbtjänsten utbildning.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-120">Find the values for the *BaseLocation*, *RelativeLocation*, and *SasBlobToken*: These values are returned in the output from your call to the Training Web Service.</span></span> 
   <span data-ttu-id="7ab8e-121">![Visar resultatet av omtränings provet och värdena BaseLocation, RelativeLocation och SasBlobToken.][image6]</span><span class="sxs-lookup"><span data-stu-id="7ab8e-121">![showing the output of the retraining sample and the BaseLocation, RelativeLocation, and  SasBlobToken values.][image6]</span></span>
3. <span data-ttu-id="7ab8e-122">Uppdatera tillagda slutpunkten från bedömningsprofil webbtjänsten med den nya tränade modellen: med den exempelkoden i träna om Machine Learning-modeller via programmering, uppdatera ny slutpunkt som du har lagt till bedömningsprofil modellen med den nyligen tränade modellen från webbtjänsten utbildning.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-122">Update the added endpoint from the scoring web service with the new trained model: Using the sample code provided in the Retrain Machine Learning models programmatically, update the new endpoint you added to the scoring model with the newly trained model from the Training Web Service.</span></span>

## <a name="common-obstacles"></a><span data-ttu-id="7ab8e-123">Vanliga hinder</span><span class="sxs-lookup"><span data-stu-id="7ab8e-123">Common obstacles</span></span>
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a><span data-ttu-id="7ab8e-124">Kontrollera om du har rätt korrigering URL</span><span class="sxs-lookup"><span data-stu-id="7ab8e-124">Check to see if you have the correct PATCH URL</span></span>
<span data-ttu-id="7ab8e-125">KORRIGERING av Webbadressen som du använder måste vara den som är kopplade till den nya bedömningsprofil slutpunkt som du lagt till bedömningsprofil webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-125">The PATCH URL you are using must be the one associated with the new scoring endpoint you added to the scoring Web service.</span></span> <span data-ttu-id="7ab8e-126">Det finns ett antal sätt att hämta URL: en korrigering:</span><span class="sxs-lookup"><span data-stu-id="7ab8e-126">There are a number of ways to obtain the PATCH URL:</span></span>

<span data-ttu-id="7ab8e-127">**Alternativ 1: genom att programmera**</span><span class="sxs-lookup"><span data-stu-id="7ab8e-127">**Option 1: Programatically**</span></span>

<span data-ttu-id="7ab8e-128">Hämta rätt korrigering URL:</span><span class="sxs-lookup"><span data-stu-id="7ab8e-128">To get the correct PATCH URL:</span></span>

1. <span data-ttu-id="7ab8e-129">Kör den [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) exempelkod.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-129">Run the [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>
2. <span data-ttu-id="7ab8e-130">Utdata från AddEndpoint, Sök efter den *HelpLocation* värdet och kopiera Webbadressen.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-130">From the output of AddEndpoint, find the *HelpLocation* value and copy the URL.</span></span>
   
   ![HelpLocation i utdata addEndpoint-exemplet.][image2]
3. <span data-ttu-id="7ab8e-132">Klistra in Webbadressen i en webbläsare för att navigera till en sida som innehåller hjälplänkar för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-132">Paste the URL into a browser to navigate to a page that provides help links for the Web service.</span></span>
4. <span data-ttu-id="7ab8e-133">Klicka på den **uppdatering resurs** länk för att öppna sidan korrigering hjälp.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-133">Click the **Update Resource** link to open the patch help page.</span></span>

<span data-ttu-id="7ab8e-134">**Alternativ 2: Använd den klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="7ab8e-134">**Option 2: Use the Azure classic portal**</span></span>

1. <span data-ttu-id="7ab8e-135">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7ab8e-135">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="7ab8e-136">Öppna fliken Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-136">Open the Machine Learning tab.</span></span> 
   <span data-ttu-id="7ab8e-137">![Datorn lutande fliken.][image4]</span><span class="sxs-lookup"><span data-stu-id="7ab8e-137">![Machine leaning tab.][image4]</span></span>
3. <span data-ttu-id="7ab8e-138">Klicka på arbetsytans namn, sedan **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-138">Click your workspace name, then **Web Services**.</span></span>
4. <span data-ttu-id="7ab8e-139">Klicka på bedömningsprofil webbtjänsten som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-139">Click the scoring Web service you are working with.</span></span> <span data-ttu-id="7ab8e-140">(Om du inte ändrar standardnamnet på webbtjänsten, det går ut om [bedömningen Exp.].)</span><span class="sxs-lookup"><span data-stu-id="7ab8e-140">(If you did not modify the default name of the web service, it will end in [Scoring Exp.].)</span></span>
5. <span data-ttu-id="7ab8e-141">Klicka på **lägga till slutpunkten**.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-141">Click **Add Endpoint**.</span></span>
6. <span data-ttu-id="7ab8e-142">När slutpunkten har lagts till, klickar du på namnet på slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-142">After the endpoint is added, click the endpoint name.</span></span> <span data-ttu-id="7ab8e-143">Klicka på **uppdatering resurs** att öppna sidan uppdatering hjälp.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-143">Then click **Update Resource** to open the patching help page.</span></span>

> [!NOTE]
> <span data-ttu-id="7ab8e-144">Om du har lagt till slutpunkten för utbildning Web Service i stället för förutsägande webbtjänsten, du får följande felmeddelande när du klickar på den **uppdatering resurs** länk: tyvärr, men den här funktionen kan inte stöds i den här kontexten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-144">If you have added the endpoint to the Training Web Service instead of the Predictive Web Service, you will receive the following error when you click the **Update Resource** link: Sorry, but this feature is not supported or available in this context.</span></span> <span data-ttu-id="7ab8e-145">Den här webbtjänsten har inga resurser för uppdateras.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-145">This Web Service has no updatable resources.</span></span> <span data-ttu-id="7ab8e-146">Vi ber om ursäkt för besväret och arbetar med att förbättra det här arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-146">We apologize for the inconvenience and are working on improving this workflow.</span></span>
> 
> 

![Ny slutpunkt instrumentpanel.][image3]

<span data-ttu-id="7ab8e-148">Hjälpsidan korrigering innehåller korrigering URL: en måste du använda och innehåller exempelkod som du kan använda för att anropa den.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-148">The PATCH help page contains the PATCH URL you must use and provides sample code you can use to call it.</span></span>

![Patch-URL.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a><span data-ttu-id="7ab8e-150">Se till att du uppdaterar korrekt bedömningsprofil slutpunkt</span><span class="sxs-lookup"><span data-stu-id="7ab8e-150">Check to see that you are updating the correct scoring endpoint</span></span>
* <span data-ttu-id="7ab8e-151">Inte korrigering webbtjänsten utbildning: patch-åtgärden måste utföras på bedömningsprofil webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-151">Do not patch the Training Web Service: The patch operation must be performed on the scoring Web service.</span></span>
* <span data-ttu-id="7ab8e-152">Inte korrigering standardslutpunkten webbtjänsten: patch-åtgärden måste utföras på den nya bedömningsprofil webbtjänstslutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-152">Do not patch the default endpoint on Web service: The patch operation must be performed on the new scoring Web service endpoint that you added.</span></span>

<span data-ttu-id="7ab8e-153">Du kan kontrollera vilka webbtjänsten slutpunkten finns på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-153">You can verify which Web service the endpoint is on by visiting the Azure classic portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="7ab8e-154">Var noga med att du lägger till slutpunkten förutsägande webbtjänsten inte utbildning-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-154">Be sure you are adding the endpoint to the Predictive Web Service, not the Training Web Service.</span></span> <span data-ttu-id="7ab8e-155">Om du har distribuerat en utbildnings- och en förutsägbar webbtjänst korrekt, bör du se två separata webbtjänster som anges.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-155">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate Web services listed.</span></span> <span data-ttu-id="7ab8e-156">Förutsägande webbtjänsten ska avslutas med ”[förutsägande exp.]”.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-156">The Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

1. <span data-ttu-id="7ab8e-157">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7ab8e-157">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="7ab8e-158">Öppna fliken Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-158">Open the Machine Learning tab.</span></span> 
   <span data-ttu-id="7ab8e-159">![Machine learning-arbetsytan Användargränssnittet.][image4]</span><span class="sxs-lookup"><span data-stu-id="7ab8e-159">![Machine learning workspace UI.][image4]</span></span>
3. <span data-ttu-id="7ab8e-160">Välj din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-160">Select your workspace.</span></span>
4. <span data-ttu-id="7ab8e-161">Klicka på **webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-161">Click **Web Services**.</span></span>
5. <span data-ttu-id="7ab8e-162">Välj förutsägbara webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-162">Select your Predictive Web Service.</span></span>
6. <span data-ttu-id="7ab8e-163">Kontrollera att din nya slutpunkt har lagts till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-163">Verify that your new endpoint was added to the Web service.</span></span>

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a><span data-ttu-id="7ab8e-164">Kontrollera arbetsytan som webbtjänsten i ska se till att den är i rätt region</span><span class="sxs-lookup"><span data-stu-id="7ab8e-164">Check the workspace that your web service is in to ensure it is in the correct region</span></span>
1. <span data-ttu-id="7ab8e-165">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="7ab8e-165">Sign in to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="7ab8e-166">Välj Machine Learning på menyn.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-166">Select Machine Learning from the menu.</span></span>
   <span data-ttu-id="7ab8e-167">![Machine learning region Användargränssnittet.][image4]</span><span class="sxs-lookup"><span data-stu-id="7ab8e-167">![Machine learning region UI.][image4]</span></span>
3. <span data-ttu-id="7ab8e-168">Kontrollera platsen för din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="7ab8e-168">Verify the location of your workspace.</span></span>

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
