---
title: "aaaConsume en Machine Learning-webbtjänst med en mall för appen | Microsoft Docs"
description: "Använd en mall för app i Azure Marketplace tooconsume förutsägande webbtjänst i Azure Machine Learning."
keywords: "maskininlärning för webbtjänst, operationalization REST API"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1199377bead470807d58ca7f7a667175cbb88450
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a><span data-ttu-id="1195f-104">Använda en Azure Machine Learning-webbtjänst med en webbappmall</span><span class="sxs-lookup"><span data-stu-id="1195f-104">Consume an Azure Machine Learning web service with a web app template</span></span>

<span data-ttu-id="1195f-105">När du har utvecklat din förutsägelsemodell och distribueras som en Azure-webbtjänst med hjälp av Machine Learning Studio eller med verktyg som R eller Python kan du komma åt hello operationalized modellen med hjälp av REST-API.</span><span class="sxs-lookup"><span data-stu-id="1195f-105">Once you've developed your predictive model and deployed it as an Azure web service using Machine Learning Studio, or using tools such as R or Python, you can access hello operationalized model using a REST API.</span></span>

<span data-ttu-id="1195f-106">Det finns ett antal sätt tooconsume hello REST-API och åtkomst hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="1195f-106">There are a number of ways tooconsume hello REST API and access hello web service.</span></span> <span data-ttu-id="1195f-107">Exempelvis kan du skriva ett program i C#, R eller Python med hello exempelkod som skapas automatiskt när du distribuerade hello-webbtjänst (tillgänglig i hello [Machine Learning Web Services-portalen](https://services.azureml.net/quickstart) eller i hello web service instrumentpanelen i Machine Learning Studio).</span><span class="sxs-lookup"><span data-stu-id="1195f-107">For example, you can write an application in C#, R, or Python using hello sample code generated for you when you deployed hello web service (available in hello [Machine Learning Web Services Portal](https://services.azureml.net/quickstart) or in hello web service dashboard in Machine Learning Studio).</span></span> <span data-ttu-id="1195f-108">Du kan också använda hello exempel Microsoft Excel-arbetsbok på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="1195f-108">Or you can use hello sample Microsoft Excel workbook created for you at hello same time.</span></span>

<span data-ttu-id="1195f-109">Men hello snabbaste och enklaste sättet tooaccess webbtjänsten är via hello Web App mallar som är tillgängliga i hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span><span class="sxs-lookup"><span data-stu-id="1195f-109">But hello quickest and easiest way tooaccess your web service is through hello Web App Templates available in hello [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-azure-machine-learning-web-app-templates"></a><span data-ttu-id="1195f-110">hello mallar för Azure Machine Learning-App</span><span class="sxs-lookup"><span data-stu-id="1195f-110">hello Azure Machine Learning Web App Templates</span></span>
<span data-ttu-id="1195f-111">hello web app mallar som är tillgängliga i hello Azure Marketplace kan skapa ett anpassat webbprogram som känner din webbtjänsten indata och förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="1195f-111">hello web app templates available in hello Azure Marketplace can build a custom web app that knows your web service's input data and expected results.</span></span> <span data-ttu-id="1195f-112">Allt du behöver toodo är att ge hello app åtkomst tooyour webbtjänst och data och hello mallen hello rest.</span><span class="sxs-lookup"><span data-stu-id="1195f-112">All you need toodo is give hello web app access tooyour web service and data, and hello template does hello rest.</span></span>

<span data-ttu-id="1195f-113">Två mallar är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="1195f-113">Two templates are available:</span></span>

* [<span data-ttu-id="1195f-114">Azure ML-svar på begäranden tjänstmall Web App</span><span class="sxs-lookup"><span data-stu-id="1195f-114">Azure ML Request-Response Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [<span data-ttu-id="1195f-115">Azure ML-Batch Execution Service Web App mall</span><span class="sxs-lookup"><span data-stu-id="1195f-115">Azure ML Batch Execution Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

<span data-ttu-id="1195f-116">Varje mall skapar ett exempel ASP.NET-program med hjälp av hello API-URI och nyckel för webbtjänsten, och distribuerar den som en webbplats tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1195f-116">Each template creates a sample ASP.NET application, using hello API URI and Key for your web service, and deploys it as a web site tooAzure.</span></span> <span data-ttu-id="1195f-117">hello skapas svar på begäranden tjänsten (RR) en webbapp som du kan använda toosend en enda rad med data toohello web service tooget ett enskilt resultat.</span><span class="sxs-lookup"><span data-stu-id="1195f-117">hello Request-Response Service (RRS) template creates a web app that allows you toosend a single row of data toohello web service tooget a single result.</span></span> <span data-ttu-id="1195f-118">hello Batch Execution Service BES-mallen skapar ett webbprogram som du kan använda toosend många rader med data tooget flera resultat.</span><span class="sxs-lookup"><span data-stu-id="1195f-118">hello Batch Execution Service (BES) template creates a web app that allows you toosend many rows of data tooget multiple results.</span></span>

<span data-ttu-id="1195f-119">Ingen kodning är obligatoriska toouse dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="1195f-119">No coding is required toouse these templates.</span></span> <span data-ttu-id="1195f-120">Du ange bara hello API-nyckel och URI och hello mallen skapar hello program åt dig.</span><span class="sxs-lookup"><span data-stu-id="1195f-120">You just supply hello API Key and URI, and hello template builds hello application for you.</span></span>

<span data-ttu-id="1195f-121">tooget hello API-nyckel och Begärd URI för en webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="1195f-121">tooget hello API key and Request URI for a web service:</span></span>

1. <span data-ttu-id="1195f-122">I hello [Web Services-portalen](https://services.azureml.net/quickstart), en ny webbtjänst klickar du på **Web Services** hello överst.</span><span class="sxs-lookup"><span data-stu-id="1195f-122">In hello [Web Services Portal](https://services.azureml.net/quickstart), for a New web service, click **Web Services** at hello top.</span></span> <span data-ttu-id="1195f-123">Eller för en klassisk web service klickar du på **klassiska webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="1195f-123">Or for a Classic web service click **Classic Web Services**.</span></span>
2. <span data-ttu-id="1195f-124">Klicka på hello-webbtjänst som du vill tooaccess.</span><span class="sxs-lookup"><span data-stu-id="1195f-124">Click hello web service you want tooaccess.</span></span>
3. <span data-ttu-id="1195f-125">Klicka på hello-slutpunkt som du vill använda tooaccess för klassisk-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="1195f-125">For a Classic web service, click hello endpoint you want tooaccess.</span></span>
4. <span data-ttu-id="1195f-126">Klicka på **förbruka** hello överst.</span><span class="sxs-lookup"><span data-stu-id="1195f-126">Click **Consume** at hello top.</span></span>
5. <span data-ttu-id="1195f-127">Kopiera hello **primära** eller **sekundärnyckeln** och spara den.</span><span class="sxs-lookup"><span data-stu-id="1195f-127">Copy hello **Primary** or **Secondary Key** and save it.</span></span>
6. <span data-ttu-id="1195f-128">Om du skapar en mall för svar på begäranden tjänsten (RR) kan du kopiera hello **begäran och svar** URI och spara den.</span><span class="sxs-lookup"><span data-stu-id="1195f-128">If you're creating a Request-Response Service (RRS) template, copy hello **Request-Response** URI and save it.</span></span> <span data-ttu-id="1195f-129">Om du skapar en mall för Batch Execution Service BES-, kopiera hello **gruppbegäranden** URI och spara den.</span><span class="sxs-lookup"><span data-stu-id="1195f-129">If you're creating a Batch Execution Service (BES) template, copy hello **Batch Requests** URI and save it.</span></span>


## <a name="how-toouse-hello-request-response-service-rrs-template"></a><span data-ttu-id="1195f-130">Hur toouse hello svar på begäranden tjänsten (RR) mall</span><span class="sxs-lookup"><span data-stu-id="1195f-130">How toouse hello Request-Response Service (RRS) template</span></span>
<span data-ttu-id="1195f-131">Följ dessa steg toouse hello Resursposter web app mall, som visas i följande diagram hello.</span><span class="sxs-lookup"><span data-stu-id="1195f-131">Follow these steps toouse hello RRS web app template, as shown in hello following diagram.</span></span>

![Web processmall toouse Resursposter][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. <span data-ttu-id="1195f-133">Gå toohello [Azure-portalen](https://portal.azure.com), **inloggning**, klickar du på **ny**, söka efter och välj **Azure ML-svar på begäranden Service Web App**, klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1195f-133">Go toohello [Azure portal](https://portal.azure.com), **Login**, click **New**, search for and select **Azure ML Request-Response Service Web App**, then click **Create**.</span></span> 
   
   * <span data-ttu-id="1195f-134">Ge ett unikt namn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1195f-134">Give your web app a unique name.</span></span> <span data-ttu-id="1195f-135">hello-URL för hello webbprogrammet blir namnet följt av `.azurewebsites.net.` t.ex.`http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="1195f-135">hello URL of hello web app will be this name followed by `.azurewebsites.net.` For example, `http://carprediction.azurewebsites.net.`</span></span>
   * <span data-ttu-id="1195f-136">Välj hello Azure-prenumeration och tjänster som webbtjänsten körs under.</span><span class="sxs-lookup"><span data-stu-id="1195f-136">Select hello Azure subscription and services under which your web service is running.</span></span>
   * <span data-ttu-id="1195f-137">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="1195f-137">Click **Create**.</span></span>
     
     ![Skapa webbapp][image5]

4. <span data-ttu-id="1195f-139">När Azure distribution hello webbprogrammet har slutförts, klickar du på hello **URL** på hello inställningssidan för web app i Azure, eller ange hello URL i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1195f-139">When Azure has finished deploying hello web app, click hello **URL** on hello web app settings page in Azure, or enter hello URL in a web browser.</span></span> <span data-ttu-id="1195f-140">Till exempel, `http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="1195f-140">For example, `http://carprediction.azurewebsites.net.`</span></span>
5. <span data-ttu-id="1195f-141">När hello web app första körs den ber dig om hello **API Post URL** och **API-nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="1195f-141">When hello web app first runs it will ask you for hello **API Post URL** and **API Key**.</span></span>
   <span data-ttu-id="1195f-142">Ange hello-värden som du sparade tidigare (**Begärd URI** och **API-nyckel**respektive).</span><span class="sxs-lookup"><span data-stu-id="1195f-142">Enter hello values you saved earlier (**Request URI** and **API Key**, respectively).</span></span>
     
     <span data-ttu-id="1195f-143">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="1195f-143">Click **Submit**.</span></span>
     
     ![Ange Post URI och API-nyckel][image6]

6. <span data-ttu-id="1195f-145">Hej web app visar dess **Webbappkonfigurationen** sidan med hello aktuella webbtjänstinställningar.</span><span class="sxs-lookup"><span data-stu-id="1195f-145">hello web app displays its **Web App Configuration** page with hello current web service settings.</span></span> <span data-ttu-id="1195f-146">Här kan du ändra toohello inställningarna som används av hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1195f-146">Here you can make changes toohello settings used by hello web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="1195f-147">Hello inställningar här endast ändras dem för det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1195f-147">Changing hello settings here only changes them for this web app.</span></span> <span data-ttu-id="1195f-148">Hello standardinställningarna för webbtjänsten ändras inte.</span><span class="sxs-lookup"><span data-stu-id="1195f-148">It doesn't change hello default settings of your web service.</span></span> <span data-ttu-id="1195f-149">Till exempel om du ändrar hello **beskrivning** här ändringen inte hello beskrivning visas på hello web service instrumentpanelen i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="1195f-149">For example, if you change hello **Description** here it doesn't change hello description shown on hello web service dashboard in Machine Learning Studio.</span></span>
   > 
   > 
   
    <span data-ttu-id="1195f-150">När du är klar klickar du på **spara ändringar**, och klicka sedan på **gå tooHome sidan**.</span><span class="sxs-lookup"><span data-stu-id="1195f-150">When you're done, click **Save changes**, and then click **Go tooHome Page**.</span></span>

7. <span data-ttu-id="1195f-151">Startsidan kan du ange värden från hello toosend tooyour-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="1195f-151">From hello home page you can enter values toosend tooyour web service.</span></span> <span data-ttu-id="1195f-152">Klicka på **skicka** när du är klar och hello resultat returneras.</span><span class="sxs-lookup"><span data-stu-id="1195f-152">Click **Submit** when you're done, and hello result will be returned.</span></span>

<span data-ttu-id="1195f-153">Om du vill tooreturn toohello **Configuration** sidan finns toohello `setting.aspx` sidan av hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1195f-153">If you want tooreturn toohello **Configuration** page, go toohello `setting.aspx` page of hello web app.</span></span> <span data-ttu-id="1195f-154">Till exempel: `http://carprediction.azurewebsites.net/setting.aspx.` du kommer att tillfrågas tooenter hello API-nyckeln igen – du behöver att tooaccess hello sidan och uppdatera inställningarna för hello.</span><span class="sxs-lookup"><span data-stu-id="1195f-154">For example: `http://carprediction.azurewebsites.net/setting.aspx.` You will be prompted tooenter hello API key again - you need that tooaccess hello page and update hello settings.</span></span>

<span data-ttu-id="1195f-155">Du kan stoppa, starta om eller ta bort hello webbprogram i hello Azure-portalen som andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="1195f-155">You can stop, restart, or delete hello web app in hello Azure portal like any other web app.</span></span> <span data-ttu-id="1195f-156">Du kan bläddra toohello hem webbadressen och ange nya värden så länge som den körs.</span><span class="sxs-lookup"><span data-stu-id="1195f-156">As long as it is running you can browse toohello home web address and enter new values.</span></span>

## <a name="how-toouse-hello-batch-execution-service-bes-template"></a><span data-ttu-id="1195f-157">Hur toouse hello Batch Execution Service BES-mall</span><span class="sxs-lookup"><span data-stu-id="1195f-157">How toouse hello Batch Execution Service (BES) template</span></span>
<span data-ttu-id="1195f-158">Du kan använda hello BES web app mall i hello samma sätt som hello RR-mall, förutom att hello-webbprogram som har skapats kan du toosubmit flera rader med data och ta emot flera resultat.</span><span class="sxs-lookup"><span data-stu-id="1195f-158">You can use hello BES web app template in hello same way as hello RRS template, except that hello web app that's created will allow you toosubmit multiple rows of data and receive multiple results.</span></span>

<span data-ttu-id="1195f-159">hello indatavärden för en webbtjänst för batch-körningen kan komma från Azure-lagring eller en lokal fil. hello resultat lagras i en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1195f-159">hello input values for a batch execution web service can come from Azure storage or a local file; hello results are stored in an Azure storage container.</span></span>
<span data-ttu-id="1195f-160">Därför ska du behöver ett Azure storage-behållare toohold hello resultaten som returnerades av hello webbprogram och du behöver tooget dina indata redo.</span><span class="sxs-lookup"><span data-stu-id="1195f-160">So, you'll need an Azure storage container toohold hello results returned by hello web app, and you'll need tooget your input data ready.</span></span>

![Bearbeta toouse BES webbmall][image2]

1. <span data-ttu-id="1195f-162">Följ hello samma procedur toocreate hello BES webbapp som hello Resursposter mall, utom gå för[Azure ML Batch Execution Service Web Appmallen](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES-mall i Azure Marketplace och klicka på **skapa webbprogram** .</span><span class="sxs-lookup"><span data-stu-id="1195f-162">Follow hello same procedure toocreate hello BES web app as for hello RRS template, except go too[Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES template on Azure Marketplace and click **Create Web App**.</span></span>

2. <span data-ttu-id="1195f-163">toospecify där du vill att hello resultatet lagras, ange hello mål behållaren information på startsidan för hello web app.</span><span class="sxs-lookup"><span data-stu-id="1195f-163">toospecify where you want hello results stored, enter hello destination container information on hello web app home page.</span></span> <span data-ttu-id="1195f-164">Ange var hello webbprogrammet kan få hello indatavärden, antingen i en lokal fil eller en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="1195f-164">Also specify where hello web app can get hello input values, either in a local file or an Azure storage container.</span></span>
   <span data-ttu-id="1195f-165">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="1195f-165">Click **Submit**.</span></span>
   
    ![Storage-informationen][image7]

<span data-ttu-id="1195f-167">hello webbprogrammet visas en sida med jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="1195f-167">hello web app will display a page with job status.</span></span>
<span data-ttu-id="1195f-168">När hello jobbet har slutförts får du hello platsen för hello resultat i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="1195f-168">When hello job has completed you'll be given hello location of hello results in Azure blob storage.</span></span> <span data-ttu-id="1195f-169">Du kan också ha hello alternativet för att hämta hello resultat tooa lokal fil.</span><span class="sxs-lookup"><span data-stu-id="1195f-169">You also have hello option of downloading hello results tooa local file.</span></span>

## <a name="for-more-information"></a><span data-ttu-id="1195f-170">Mer information</span><span class="sxs-lookup"><span data-stu-id="1195f-170">For more information</span></span>
<span data-ttu-id="1195f-171">Mer information om toolearn...</span><span class="sxs-lookup"><span data-stu-id="1195f-171">toolearn more about...</span></span>

* <span data-ttu-id="1195f-172">Skapa ett experiment i machine learning med Machine Learning Studio finns [skapa ditt första experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md)</span><span class="sxs-lookup"><span data-stu-id="1195f-172">creating a machine learning experiment with Machine Learning Studio, see [Create your first experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)</span></span>
* <span data-ttu-id="1195f-173">hur toodeploy din maskininlärning experiment som en webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md)</span><span class="sxs-lookup"><span data-stu-id="1195f-173">how toodeploy your machine learning experiment as a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md)</span></span>
* <span data-ttu-id="1195f-174">andra sätt tooaccess webbtjänsten, se [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="1195f-174">other ways tooaccess your web service, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span></span>

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
