---
title: "aaaExcel-tillägg för webbtjänster för Machine Learning | Microsoft Docs"
description: "Hur toouse Azure Machine Learning Web services direkt i Excel utan att skriva någon kod."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: c52f40d33c9907f284e4750afe47181dc3365fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a><span data-ttu-id="ec2bd-103">Excel-tillägg för Azure Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="ec2bd-103">Excel Add-in for Azure Machine Learning web services</span></span>
<span data-ttu-id="ec2bd-104">Excel gör det enkelt toocall webbtjänster direkt utan hello måste toowrite någon kod.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-104">Excel makes it easy toocall web services directly without hello need toowrite any code.</span></span>

## <a name="steps-toouse-an-existing-web-service-in-hello-workbook"></a><span data-ttu-id="ec2bd-105">Steg tooUse en befintlig webbtjänst i hello arbetsboken</span><span class="sxs-lookup"><span data-stu-id="ec2bd-105">Steps tooUse an Existing web service in hello Workbook</span></span>

1. <span data-ttu-id="ec2bd-106">Öppna hello [Excel exempelfilen](http://aka.ms/amlexcel-sample-2), som innehåller hello Excel-tillägget och data om passagerare på hello Titanic.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-106">Open hello [sample Excel file](http://aka.ms/amlexcel-sample-2), which contains hello Excel add-in and data about passengers on hello Titanic.</span></span>
2. <span data-ttu-id="ec2bd-107">Välj hello-webbtjänsten genom att klicka på den-”Titanic efterlevande ge prognoser (Excel-tillägg prov) [poäng]” i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-107">Choose hello web service by clicking it - "Titanic Survivor Predictor (Excel Add-in Sample) [Score]" in this example.</span></span>
   
    ![Välj-webbtjänst][01]
3. <span data-ttu-id="ec2bd-109">Detta tar toohello **Predict** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-109">This takes you toohello **Predict** section.</span></span>  <span data-ttu-id="ec2bd-110">Den här arbetsboken innehåller redan exempeldata, men en tom arbetsbok du kan markera en cell i Excel och klickar på **använda exempeldata**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-110">This workbook already contains sample data, but for a blank workbook you can select a cell in Excel and click **Use sample data**.</span></span>
4. <span data-ttu-id="ec2bd-111">Välj hello data med rubriker och klickar på ikonen för hello indata intervall.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-111">Select hello data with headers and click hello input data range icon.</span></span>  <span data-ttu-id="ec2bd-112">Kontrollera hello ”Mina data har rubriker” kryssrutan är markerad.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-112">Make sure hello "My data has headers" box is checked.</span></span>
5. <span data-ttu-id="ec2bd-113">Under **utdata**, ange antalet celler för hello där du vill hello utdata toobe, till exempel ”H1” här.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-113">Under **Output**, enter hello cell number where you want hello output toobe, for example "H1" here.</span></span>
6. <span data-ttu-id="ec2bd-114">Klicka på **förutsäga**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-114">Click **Predict**.</span></span>
   
    ![Förutsäga avsnitt][02]

<span data-ttu-id="ec2bd-116">Distribuera en webbtjänst eller Använd en befintlig webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-116">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="ec2bd-117">Mer information om hur du distribuerar en webbtjänst finns [genomgången steg 5: distribuera hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="ec2bd-117">For more information on deploying a web service, see [Walkthrough Step 5: Deploy hello Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>

<span data-ttu-id="ec2bd-118">Hämta hello API-nyckel för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-118">Get hello API key for your web service.</span></span> <span data-ttu-id="ec2bd-119">Om du utför beror den här åtgärden på om du har publicerat en klassiska Machine Learning-webbtjänst för en ny Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-119">Where you perform this action depends on whether you published a Classic Machine Learning web service of a New Machine Learning web service.</span></span>

<span data-ttu-id="ec2bd-120">**Använda en klassisk-webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="ec2bd-120">**Use a Classic web service**</span></span> 

1. <span data-ttu-id="ec2bd-121">Klicka på hello i Machine Learning Studio **WEB SERVICES** avsnittet hello vänster och väljer sedan hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-121">In Machine Learning Studio, click hello **WEB SERVICES** section in hello left pane, and then select hello web service.</span></span>
   
    ![Studio väljer du en webbtjänst][04]
2. <span data-ttu-id="ec2bd-123">Kopiera hello API-nyckel för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-123">Copy hello API key for hello web service.</span></span>
   
    ![Studio API-nyckel][05]
3. <span data-ttu-id="ec2bd-125">På hello **INSTRUMENTPANELEN** för hello webbtjänsten klickar du på hello **frågor och svar** länk.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-125">On hello **DASHBOARD** tab for hello web service, click hello **REQUEST/RESPONSE** link.</span></span>
4. <span data-ttu-id="ec2bd-126">Leta efter hello **Begärd URI** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-126">Look for hello **Request URI** section.</span></span>  <span data-ttu-id="ec2bd-127">Kopiera och spara hello-URL.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-127">Copy and save hello URL.</span></span>

> [!NOTE]
> <span data-ttu-id="ec2bd-128">Det är nu möjligt toosign till hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal tooobtain hello API-nyckel för en klassiska Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-128">It is now possible toosign into hello [Azure Machine Learning Web Services](https://services.azureml.net) portal tooobtain hello API key for a Classic Machine Learning web service.</span></span>
> 
> 

<span data-ttu-id="ec2bd-129">**Använd en ny webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="ec2bd-129">**Use a New web service**</span></span>

1. <span data-ttu-id="ec2bd-130">I hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) klickar du på **Web Services**, Välj din webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-130">In hello [Azure Machine Learning Web Services](https://services.azureml.net) portal, click **Web Services**, then select your web service.</span></span> 
2. <span data-ttu-id="ec2bd-131">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-131">Click **Consume**.</span></span>
3. <span data-ttu-id="ec2bd-132">Leta efter hello **grundläggande förbrukning info** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-132">Look for hello **Basic consumption info** section.</span></span> <span data-ttu-id="ec2bd-133">Kopiera och spara hello **primärnyckel** och hello **begäran och svar** URL.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-133">Copy and save hello **Primary Key** and hello **Request-Response** URL.</span></span>

## <a name="steps-tooadd-a-new-web-service"></a><span data-ttu-id="ec2bd-134">Steg tooAdd en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ec2bd-134">Steps tooAdd a New web service</span></span>

1. <span data-ttu-id="ec2bd-135">Distribuera en webbtjänst eller Använd en befintlig webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-135">Deploy a web service or use an existing Web service.</span></span> <span data-ttu-id="ec2bd-136">Mer information om hur du distribuerar en webbtjänst finns [genomgången steg 5: distribuera hello Azure Machine Learning-webbtjänst](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="ec2bd-136">For more information on deploying a web service, see [Walkthrough Step 5: Deploy hello Azure Machine Learning Web service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
2. <span data-ttu-id="ec2bd-137">Klicka på **använda**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-137">Click **Consume**.</span></span>
3. <span data-ttu-id="ec2bd-138">Leta efter hello **grundläggande förbrukning info** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-138">Look for hello **Basic consumption info** section.</span></span> <span data-ttu-id="ec2bd-139">Kopiera och spara hello **primärnyckel** och hello **begäran och svar** URL.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-139">Copy and save hello **Primary Key** and hello **Request-Response** URL.</span></span>
4. <span data-ttu-id="ec2bd-140">I Excel, går toohello **Web Services** avsnittet (om du är i hello **Predict** klickar du på Bakåt pilen hello toogo toohello lista över webbtjänster).</span><span class="sxs-lookup"><span data-stu-id="ec2bd-140">In Excel, go toohello **Web Services** section (if you are in hello **Predict** section, click hello back arrow toogo toohello list of web services).</span></span>
   
    ![Gå tooWeb service markering][03]
5. <span data-ttu-id="ec2bd-142">Klicka på **Lägg till webbtjänst**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-142">Click **Add Web Service**.</span></span>
6. <span data-ttu-id="ec2bd-143">Klistra in hello URL i hello Excel-tillägget textruta med etiketten **URL**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-143">Paste hello URL into hello Excel add-in text box labeled **URL**.</span></span>
7. <span data-ttu-id="ec2bd-144">Klistra in hello API eller den primära nyckeln i hello textrutan **API-nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-144">Paste hello API/Primary key into hello text box labeled **API key**.</span></span>
8. <span data-ttu-id="ec2bd-145">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-145">Click **Add**.</span></span>
   
    ![URL: en och API-nyckeln för det klassiska.][06]
9. <span data-ttu-id="ec2bd-147">webbtjänst för toouse hello, följer du föregående anvisningar hello, ”steg tooUse en befintlig web Service”.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-147">toouse hello web service, follow hello preceding directions, "Steps tooUse an Existing web Service."</span></span>

## <a name="sharing-your-workbook"></a><span data-ttu-id="ec2bd-148">Dela din arbetsbok</span><span class="sxs-lookup"><span data-stu-id="ec2bd-148">Sharing Your Workbook</span></span>
<span data-ttu-id="ec2bd-149">Om du sparar arbetsboken sparas också hello API eller den primära nyckeln för hello webbtjänster som du har lagt till.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-149">If you save your workbook, then hello API/Primary key for hello web services you have added is also saved.</span></span> <span data-ttu-id="ec2bd-150">Det innebär att du ska dela hello arbetsboken med personer som du litar på.</span><span class="sxs-lookup"><span data-stu-id="ec2bd-150">That means you should only share hello workbook with individuals you trust.</span></span>

<span data-ttu-id="ec2bd-151">Frågor i hello följande kommentar avsnittet eller på vår [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="ec2bd-151">Ask any questions in hello following comment section or on our [forum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).</span></span>

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
