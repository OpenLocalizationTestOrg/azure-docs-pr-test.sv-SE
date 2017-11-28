---
title: "aaaTroubleshoot omtränings en Azure Machine Learning klassiska webbtjänsten | Microsoft Docs"
description: "Identifiera och lösa vanliga problem uppstod när du omtränings hello modellen för en Azure Machine Learning-webbtjänst."
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
ms.openlocfilehash: 2b6a78eaba161877106dccdc23437b5e454fca7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-retraining-of-an-azure-machine-learning-classic-web-service"></a><span data-ttu-id="9a679-103">Felsöka hello omtränings av en Azure Machine Learning klassiska-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="9a679-103">Troubleshooting hello retraining of an Azure Machine Learning Classic Web service</span></span>
## <a name="retraining-overview"></a><span data-ttu-id="9a679-104">Omtränings översikt</span><span class="sxs-lookup"><span data-stu-id="9a679-104">Retraining overview</span></span>
<span data-ttu-id="9a679-105">När du distribuerar en prediktivt experiment som en bedömningsprofil webbtjänst är en statisk modell.</span><span class="sxs-lookup"><span data-stu-id="9a679-105">When you deploy a predictive experiment as a scoring web service it is a static model.</span></span> <span data-ttu-id="9a679-106">När nya data blir tillgängliga eller när hello konsumenter av hello API har sina egna data, måste hello modellen toobe retrained.</span><span class="sxs-lookup"><span data-stu-id="9a679-106">As new data becomes available or when hello consumer of hello API has their own data, hello model needs toobe retrained.</span></span> 

<span data-ttu-id="9a679-107">En fullständig genomgång av hello omtränings av en klassiska webbtjänst finns [träna om Machine Learning-modeller via programmering](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="9a679-107">For a complete walkthrough of hello retraining process of a Classic Web service, see [Retrain Machine Learning Models Programmatically](machine-learning-retrain-models-programmatically.md).</span></span>

## <a name="retraining-process"></a><span data-ttu-id="9a679-108">Omtränings process</span><span class="sxs-lookup"><span data-stu-id="9a679-108">Retraining process</span></span>
<span data-ttu-id="9a679-109">När du behöver tooretrain hello webbtjänsten måste du lägga till vissa ytterligare delar:</span><span class="sxs-lookup"><span data-stu-id="9a679-109">When you need tooretrain hello Web service, you must add some additional pieces:</span></span>

* <span data-ttu-id="9a679-110">En webbtjänst som distribueras från hello Träningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="9a679-110">A Web service deployed from hello Training Experiment.</span></span> <span data-ttu-id="9a679-111">hello experiment måste ha en **Web Service utdata** modulen ansluten toohello utdata från hello **Träningsmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="9a679-111">hello experiment must have a **Web Service Output** module attached toohello output of hello **Train Model** module.</span></span>  
  
    ![Koppla hello web service utdata toohello train-modell.][image1]
* <span data-ttu-id="9a679-113">En ny slutpunkt läggs tooyour poängsättning av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-113">A new endpoint added tooyour scoring Web service.</span></span>  <span data-ttu-id="9a679-114">Du kan lägga till hello endpoint programmässigt med hello exempelkod som refereras i hello träna om Machine Learning-modeller via programmering avsnittet eller via hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a679-114">You can add hello endpoint programmatically using hello sample code referenced in hello Retrain Machine Learning models programmatically topic or through hello Azure classic portal.</span></span>

<span data-ttu-id="9a679-115">Du kan sedan använda hello C# exempelkod från hello utbildning Web tjänstens API hjälp sidan tooretrain modell.</span><span class="sxs-lookup"><span data-stu-id="9a679-115">You can then use hello sample C# code from hello Training Web Service's API help page tooretrain model.</span></span> <span data-ttu-id="9a679-116">När du har utvärderat hello resultat och är nöjd med dem kan uppdatera du hello tränade modellen bedömningen webbtjänsten med hello ny slutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="9a679-116">Once you have evaluated hello results and are satisfied with them, you update hello trained model scoring web service using hello new endpoint that you added.</span></span>

<span data-ttu-id="9a679-117">Med alla hello delar på plats är hello viktiga steg du måste vidta tooretrain hello modellen följande:</span><span class="sxs-lookup"><span data-stu-id="9a679-117">With all hello pieces in place, hello major steps you must take tooretrain hello model are as follows:</span></span>

1. <span data-ttu-id="9a679-118">Anropa hello utbildning webbtjänst: hello anropet är toohello Batch Execution Service BES-, inte hello Begär svar Service (RR).</span><span class="sxs-lookup"><span data-stu-id="9a679-118">Call hello Training Web Service:  hello call is toohello Batch Execution Service (BES), not hello Request Response Service (RRS).</span></span> <span data-ttu-id="9a679-119">Du kan använda hello C# exempelkoden i hello API hjälp sidan toomake hello anropet.</span><span class="sxs-lookup"><span data-stu-id="9a679-119">You can use hello sample C# code on hello API help page toomake hello call.</span></span> 
2. <span data-ttu-id="9a679-120">Hitta hello värden för hello *BaseLocation*, *RelativeLocation*, och *SasBlobToken*: dessa värden returneras i hello utdata från din anropet toohello utbildning Tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-120">Find hello values for hello *BaseLocation*, *RelativeLocation*, and *SasBlobToken*: These values are returned in hello output from your call toohello Training Web Service.</span></span> 
   <span data-ttu-id="9a679-121">![Visar hello utdata från hello omtränings exemplet och hello BaseLocation, RelativeLocation och SasBlobToken värden.][image6]</span><span class="sxs-lookup"><span data-stu-id="9a679-121">![showing hello output of hello retraining sample and hello BaseLocation, RelativeLocation, and  SasBlobToken values.][image6]</span></span>
3. <span data-ttu-id="9a679-122">Uppdateringen hello läggs endpoint från hello bedömningen webbtjänst med hello nya tränats modellen: med hello exempelkod anges i hello träna om Machine Learning modeller via programmering, uppdatera hello ny slutpunkt du har lagt till toohello bedömningen modellen med hello nyligen tränade modellen från hello utbildning webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-122">Update hello added endpoint from hello scoring web service with hello new trained model: Using hello sample code provided in hello Retrain Machine Learning models programmatically, update hello new endpoint you added toohello scoring model with hello newly trained model from hello Training Web Service.</span></span>

## <a name="common-obstacles"></a><span data-ttu-id="9a679-123">Vanliga hinder</span><span class="sxs-lookup"><span data-stu-id="9a679-123">Common obstacles</span></span>
### <a name="check-toosee-if-you-have-hello-correct-patch-url"></a><span data-ttu-id="9a679-124">Kontrollera toosee om du har hello korrigera korrigering URL</span><span class="sxs-lookup"><span data-stu-id="9a679-124">Check toosee if you have hello correct PATCH URL</span></span>
<span data-ttu-id="9a679-125">hello korrigering URL som du använder måste vara hello något som är associerade med hello nya bedömningsslutpunkten du har lagt till toohello poängsättning av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-125">hello PATCH URL you are using must be hello one associated with hello new scoring endpoint you added toohello scoring Web service.</span></span> <span data-ttu-id="9a679-126">Det finns ett antal sätt tooobtain hello korrigering URL:</span><span class="sxs-lookup"><span data-stu-id="9a679-126">There are a number of ways tooobtain hello PATCH URL:</span></span>

<span data-ttu-id="9a679-127">**Alternativ 1: genom att programmera**</span><span class="sxs-lookup"><span data-stu-id="9a679-127">**Option 1: Programatically**</span></span>

<span data-ttu-id="9a679-128">tooget hello korrigera korrigering URL:</span><span class="sxs-lookup"><span data-stu-id="9a679-128">tooget hello correct PATCH URL:</span></span>

1. <span data-ttu-id="9a679-129">Kör hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) exempelkod.</span><span class="sxs-lookup"><span data-stu-id="9a679-129">Run hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) sample code.</span></span>
2. <span data-ttu-id="9a679-130">Hitta hello från hello-utdata för AddEndpoint *HelpLocation* värde och kopiera hello-URL.</span><span class="sxs-lookup"><span data-stu-id="9a679-130">From hello output of AddEndpoint, find hello *HelpLocation* value and copy hello URL.</span></span>
   
   ![HelpLocation i hello utdata hello addEndpoint exemplet.][image2]
3. <span data-ttu-id="9a679-132">Klistra in hello URL i en webbläsare toonavigate tooa sida som innehåller hjälplänkar för hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-132">Paste hello URL into a browser toonavigate tooa page that provides help links for hello Web service.</span></span>
4. <span data-ttu-id="9a679-133">Klicka på hello **uppdatering resurs** hjälpsidan för länken tooopen hello korrigering.</span><span class="sxs-lookup"><span data-stu-id="9a679-133">Click hello **Update Resource** link tooopen hello patch help page.</span></span>

<span data-ttu-id="9a679-134">**Alternativ 2: Använd hello klassiska Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="9a679-134">**Option 2: Use hello Azure classic portal**</span></span>

1. <span data-ttu-id="9a679-135">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9a679-135">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9a679-136">Öppna hello Machine Learning-fliken. ![Datorn lutande fliken.][image4]</span><span class="sxs-lookup"><span data-stu-id="9a679-136">Open hello Machine Learning tab. ![Machine leaning tab.][image4]</span></span>
3. <span data-ttu-id="9a679-137">Klicka på arbetsytans namn, sedan **Web Services**.</span><span class="sxs-lookup"><span data-stu-id="9a679-137">Click your workspace name, then **Web Services**.</span></span>
4. <span data-ttu-id="9a679-138">Klicka på hello bedömningen webbtjänst som du arbetar med.</span><span class="sxs-lookup"><span data-stu-id="9a679-138">Click hello scoring Web service you are working with.</span></span> <span data-ttu-id="9a679-139">(Om du inte ändrar hello standardnamnet hello-webbtjänsten, det går ut om [bedömningen Exp.].)</span><span class="sxs-lookup"><span data-stu-id="9a679-139">(If you did not modify hello default name of hello web service, it will end in [Scoring Exp.].)</span></span>
5. <span data-ttu-id="9a679-140">Klicka på **lägga till slutpunkten**.</span><span class="sxs-lookup"><span data-stu-id="9a679-140">Click **Add Endpoint**.</span></span>
6. <span data-ttu-id="9a679-141">När hello slutpunkt har lagts till, klickar du på namnet på slutpunkten hello.</span><span class="sxs-lookup"><span data-stu-id="9a679-141">After hello endpoint is added, click hello endpoint name.</span></span> <span data-ttu-id="9a679-142">Klicka på **uppdatering resurs** tooopen hello hjälpsidan för uppdatering.</span><span class="sxs-lookup"><span data-stu-id="9a679-142">Then click **Update Resource** tooopen hello patching help page.</span></span>

> [!NOTE]
> <span data-ttu-id="9a679-143">Om du har lagt till hello endpoint toohello utbildning webbtjänsten i stället för hello förutsägande webbtjänsten, du får följande fel när du klickar på hello hello **uppdatering resurs** länk: tyvärr, men den här funktionen stöds inte eller tillgängligt i den här kontexten.</span><span class="sxs-lookup"><span data-stu-id="9a679-143">If you have added hello endpoint toohello Training Web Service instead of hello Predictive Web Service, you will receive hello following error when you click hello **Update Resource** link: Sorry, but this feature is not supported or available in this context.</span></span> <span data-ttu-id="9a679-144">Den här webbtjänsten har inga resurser för uppdateras.</span><span class="sxs-lookup"><span data-stu-id="9a679-144">This Web Service has no updatable resources.</span></span> <span data-ttu-id="9a679-145">Vi ber om ursäkt för besväret hello och arbetar med att förbättra det här arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="9a679-145">We apologize for hello inconvenience and are working on improving this workflow.</span></span>
> 
> 

![Ny slutpunkt instrumentpanel.][image3]

<span data-ttu-id="9a679-147">hello korrigering hjälpsidan innehåller hello korrigering URL måste du använda och exempelkod som du kan använda toocall den.</span><span class="sxs-lookup"><span data-stu-id="9a679-147">hello PATCH help page contains hello PATCH URL you must use and provides sample code you can use toocall it.</span></span>

![Patch-URL.][image5]

### <a name="check-toosee-that-you-are-updating-hello-correct-scoring-endpoint"></a><span data-ttu-id="9a679-149">Kontrollera att du uppdaterar hello korrekt bedömningsprofil slutpunkt toosee</span><span class="sxs-lookup"><span data-stu-id="9a679-149">Check toosee that you are updating hello correct scoring endpoint</span></span>
* <span data-ttu-id="9a679-150">Inte korrigering hello utbildning webbtjänst: hello korrigering åtgärden måste utföras på hello poängsättning av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-150">Do not patch hello Training Web Service: hello patch operation must be performed on hello scoring Web service.</span></span>
* <span data-ttu-id="9a679-151">Inte korrigering hello standardslutpunkten webbtjänsten: hello korrigering åtgärden måste utföras på hello nya bedömningen webbtjänstslutpunkt som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="9a679-151">Do not patch hello default endpoint on Web service: hello patch operation must be performed on hello new scoring Web service endpoint that you added.</span></span>

<span data-ttu-id="9a679-152">Du kan kontrollera vilka hello webbtjänstslutpunkt är som besökande hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9a679-152">You can verify which Web service hello endpoint is on by visiting hello Azure classic portal.</span></span> 

> [!NOTE]
> <span data-ttu-id="9a679-153">Var noga med att du lägger till hello endpoint toohello förutsägande webbtjänst, inte hello utbildning webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-153">Be sure you are adding hello endpoint toohello Predictive Web Service, not hello Training Web Service.</span></span> <span data-ttu-id="9a679-154">Om du har distribuerat en utbildnings- och en förutsägbar webbtjänst korrekt, bör du se två separata webbtjänster som anges.</span><span class="sxs-lookup"><span data-stu-id="9a679-154">If you have correctly deployed both a Training and a Predictive Web Service, you should see two separate Web services listed.</span></span> <span data-ttu-id="9a679-155">hello förutsägande webbtjänsten ska avslutas med ”[förutsägande exp.]”.</span><span class="sxs-lookup"><span data-stu-id="9a679-155">hello Predictive Web Service should end with "[predictive exp.]".</span></span>
> 
> 

1. <span data-ttu-id="9a679-156">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9a679-156">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9a679-157">Öppna hello Machine Learning-fliken. ![Machine learning-arbetsytan Användargränssnittet.][image4]</span><span class="sxs-lookup"><span data-stu-id="9a679-157">Open hello Machine Learning tab. ![Machine learning workspace UI.][image4]</span></span>
3. <span data-ttu-id="9a679-158">Välj din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="9a679-158">Select your workspace.</span></span>
4. <span data-ttu-id="9a679-159">Klicka på **webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="9a679-159">Click **Web Services**.</span></span>
5. <span data-ttu-id="9a679-160">Välj förutsägbara webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-160">Select your Predictive Web Service.</span></span>
6. <span data-ttu-id="9a679-161">Kontrollera att din nya slutpunkt har lagts till toohello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="9a679-161">Verify that your new endpoint was added toohello Web service.</span></span>

### <a name="check-hello-workspace-that-your-web-service-is-in-tooensure-it-is-in-hello-correct-region"></a><span data-ttu-id="9a679-162">Kontrollera hello-arbetsyta som webbtjänsten har tooensure i hello rätt region</span><span class="sxs-lookup"><span data-stu-id="9a679-162">Check hello workspace that your web service is in tooensure it is in hello correct region</span></span>
1. <span data-ttu-id="9a679-163">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="9a679-163">Sign in toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="9a679-164">Välj Machine Learning hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="9a679-164">Select Machine Learning from hello menu.</span></span>
   <span data-ttu-id="9a679-165">![Machine learning region Användargränssnittet.][image4]</span><span class="sxs-lookup"><span data-stu-id="9a679-165">![Machine learning region UI.][image4]</span></span>
3. <span data-ttu-id="9a679-166">Kontrollera hello platsen för din arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="9a679-166">Verify hello location of your workspace.</span></span>

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
