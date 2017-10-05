---
title: "Använda en Machine Learning-webbtjänst med en mall för appen | Microsoft Docs"
description: "Använd en mall för app i Azure Marketplace för att använda en förutsägbar webbtjänst i Azure Machine Learning."
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
ms.openlocfilehash: 95aa1fa23d83ec0dcd00870179167e803bafbd16
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a><span data-ttu-id="f98b0-104">Använda en Azure Machine Learning-webbtjänst med en webbappmall</span><span class="sxs-lookup"><span data-stu-id="f98b0-104">Consume an Azure Machine Learning web service with a web app template</span></span>

<span data-ttu-id="f98b0-105">När du har utvecklat din förutsägelsemodell och distribueras som en Azure-webbtjänst med hjälp av Machine Learning Studio eller med verktyg som R eller Python kan du komma åt operationalized modellen med hjälp av REST-API.</span><span class="sxs-lookup"><span data-stu-id="f98b0-105">Once you've developed your predictive model and deployed it as an Azure web service using Machine Learning Studio, or using tools such as R or Python, you can access the operationalized model using a REST API.</span></span>

<span data-ttu-id="f98b0-106">Det finns ett antal sätt att använda REST-API och få åtkomst till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="f98b0-106">There are a number of ways to consume the REST API and access the web service.</span></span> <span data-ttu-id="f98b0-107">Du kan till exempel skriva ett program i C#, R eller Python med exempelkoden genererade för dig när du distribuerade webbtjänsten (tillgänglig i den [Machine Learning Web Services-portalen](https://services.azureml.net/quickstart) eller i web service instrumentpanelen i Machine Learning Studio).</span><span class="sxs-lookup"><span data-stu-id="f98b0-107">For example, you can write an application in C#, R, or Python using the sample code generated for you when you deployed the web service (available in the [Machine Learning Web Services Portal](https://services.azureml.net/quickstart) or in the web service dashboard in Machine Learning Studio).</span></span> <span data-ttu-id="f98b0-108">Eller så kan du använda exemplet Microsoft Excel-arbetsbok på samma gång.</span><span class="sxs-lookup"><span data-stu-id="f98b0-108">Or you can use the sample Microsoft Excel workbook created for you at the same time.</span></span>

<span data-ttu-id="f98b0-109">Men de snabbaste och enklaste sättet att komma åt webbtjänsten är via Web App mallar som är tillgängliga i den [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span><span class="sxs-lookup"><span data-stu-id="f98b0-109">But the quickest and easiest way to access your web service is through the Web App Templates available in the [Azure Web App Marketplace](https://azure.microsoft.com/marketplace/web-applications/all/).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a><span data-ttu-id="f98b0-110">I Azure Machine Learning mallar för App</span><span class="sxs-lookup"><span data-stu-id="f98b0-110">The Azure Machine Learning Web App Templates</span></span>
<span data-ttu-id="f98b0-111">Web app tillgängliga mallar i Azure Marketplace kan skapa ett anpassat webbprogram som känner din webbtjänsten indata och förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="f98b0-111">The web app templates available in the Azure Marketplace can build a custom web app that knows your web service's input data and expected results.</span></span> <span data-ttu-id="f98b0-112">Allt du behöver göra är att ge appen webbåtkomst webbtjänsten och data och mallen gör resten.</span><span class="sxs-lookup"><span data-stu-id="f98b0-112">All you need to do is give the web app access to your web service and data, and the template does the rest.</span></span>

<span data-ttu-id="f98b0-113">Två mallar är tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="f98b0-113">Two templates are available:</span></span>

* [<span data-ttu-id="f98b0-114">Azure ML-svar på begäranden tjänstmall Web App</span><span class="sxs-lookup"><span data-stu-id="f98b0-114">Azure ML Request-Response Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [<span data-ttu-id="f98b0-115">Azure ML-Batch Execution Service Web App mall</span><span class="sxs-lookup"><span data-stu-id="f98b0-115">Azure ML Batch Execution Service Web App Template</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

<span data-ttu-id="f98b0-116">Varje mall skapar ett exempel ASP.NET-program med hjälp av API-URI och nyckel för webbtjänsten, och distribuerar den som en webbplats till Azure.</span><span class="sxs-lookup"><span data-stu-id="f98b0-116">Each template creates a sample ASP.NET application, using the API URI and Key for your web service, and deploys it as a web site to Azure.</span></span> <span data-ttu-id="f98b0-117">Svar på begäranden tjänsten (RR) skapas en webbapp som gör det möjligt att skicka en enda rad med data till webbtjänsten för att få ett enskilt resultat.</span><span class="sxs-lookup"><span data-stu-id="f98b0-117">The Request-Response Service (RRS) template creates a web app that allows you to send a single row of data to the web service to get a single result.</span></span> <span data-ttu-id="f98b0-118">Batch Execution Service BES-mallen skapar en webbapp som gör att du kan skicka många rader med data för att få flera resultat.</span><span class="sxs-lookup"><span data-stu-id="f98b0-118">The Batch Execution Service (BES) template creates a web app that allows you to send many rows of data to get multiple results.</span></span>

<span data-ttu-id="f98b0-119">Ingen kodning krävs för att använda dessa mallar.</span><span class="sxs-lookup"><span data-stu-id="f98b0-119">No coding is required to use these templates.</span></span> <span data-ttu-id="f98b0-120">Du ange bara API-nyckel och URI och mallen skapar program åt dig.</span><span class="sxs-lookup"><span data-stu-id="f98b0-120">You just supply the API Key and URI, and the template builds the application for you.</span></span>

<span data-ttu-id="f98b0-121">Hämta API-nyckel och Begärd URI för en webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="f98b0-121">To get the API key and Request URI for a web service:</span></span>

1. <span data-ttu-id="f98b0-122">I den [Web Services-portalen](https://services.azureml.net/quickstart), en ny webbtjänst klickar du på **Web Services** längst upp.</span><span class="sxs-lookup"><span data-stu-id="f98b0-122">In the [Web Services Portal](https://services.azureml.net/quickstart), for a New web service, click **Web Services** at the top.</span></span> <span data-ttu-id="f98b0-123">Eller för en klassisk web service klickar du på **klassiska webbtjänster**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-123">Or for a Classic web service click **Classic Web Services**.</span></span>
2. <span data-ttu-id="f98b0-124">Klicka på den webbtjänst som du vill komma åt.</span><span class="sxs-lookup"><span data-stu-id="f98b0-124">Click the web service you want to access.</span></span>
3. <span data-ttu-id="f98b0-125">Klicka på den slutpunkt som du vill komma åt för en klassisk webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="f98b0-125">For a Classic web service, click the endpoint you want to access.</span></span>
4. <span data-ttu-id="f98b0-126">Klicka på **förbruka** längst upp.</span><span class="sxs-lookup"><span data-stu-id="f98b0-126">Click **Consume** at the top.</span></span>
5. <span data-ttu-id="f98b0-127">Kopiera den **primära** eller **sekundärnyckeln** och spara den.</span><span class="sxs-lookup"><span data-stu-id="f98b0-127">Copy the **Primary** or **Secondary Key** and save it.</span></span>
6. <span data-ttu-id="f98b0-128">Om du skapar en mall för svar på begäranden tjänsten (RR), kopiera den **begäran och svar** URI och spara den.</span><span class="sxs-lookup"><span data-stu-id="f98b0-128">If you're creating a Request-Response Service (RRS) template, copy the **Request-Response** URI and save it.</span></span> <span data-ttu-id="f98b0-129">Om du skapar en mall för Batch Execution Service BES-, kopiera den **gruppbegäranden** URI och spara den.</span><span class="sxs-lookup"><span data-stu-id="f98b0-129">If you're creating a Batch Execution Service (BES) template, copy the **Batch Requests** URI and save it.</span></span>


## <a name="how-to-use-the-request-response-service-rrs-template"></a><span data-ttu-id="f98b0-130">Hur du använder mallen svar på begäranden tjänsten (RR)</span><span class="sxs-lookup"><span data-stu-id="f98b0-130">How to use the Request-Response Service (RRS) template</span></span>
<span data-ttu-id="f98b0-131">Följ dessa steg om du vill använda mallen Resursposter web app, som visas i följande diagram.</span><span class="sxs-lookup"><span data-stu-id="f98b0-131">Follow these steps to use the RRS web app template, as shown in the following diagram.</span></span>

![Processen för att använda Resursposter webbmall][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. <span data-ttu-id="f98b0-133">Gå till den [Azure-portalen](https://portal.azure.com), **inloggning**, klickar du på **ny**, söka efter och välj **Azure ML-svar på begäranden Service Web App**, klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-133">Go to the [Azure portal](https://portal.azure.com), **Login**, click **New**, search for and select **Azure ML Request-Response Service Web App**, then click **Create**.</span></span> 
   
   * <span data-ttu-id="f98b0-134">Ge ett unikt namn för ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f98b0-134">Give your web app a unique name.</span></span> <span data-ttu-id="f98b0-135">URL till webbprogrammet blir namnet följt av `.azurewebsites.net.` t.ex.`http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="f98b0-135">The URL of the web app will be this name followed by `.azurewebsites.net.` For example, `http://carprediction.azurewebsites.net.`</span></span>
   * <span data-ttu-id="f98b0-136">Välj Azure-prenumeration och tjänster som webbtjänsten körs under.</span><span class="sxs-lookup"><span data-stu-id="f98b0-136">Select the Azure subscription and services under which your web service is running.</span></span>
   * <span data-ttu-id="f98b0-137">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-137">Click **Create**.</span></span>
     
     ![Skapa webbapp][image5]

4. <span data-ttu-id="f98b0-139">När Azure distribution webbprogrammet har slutförts, klickar du på den **URL** på webbappsinställningarna sidan i Azure eller anger en URL i en webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f98b0-139">When Azure has finished deploying the web app, click the **URL** on the web app settings page in Azure, or enter the URL in a web browser.</span></span> <span data-ttu-id="f98b0-140">Till exempel, `http://carprediction.azurewebsites.net.`</span><span class="sxs-lookup"><span data-stu-id="f98b0-140">For example, `http://carprediction.azurewebsites.net.`</span></span>
5. <span data-ttu-id="f98b0-141">När webbappen körs första gången uppmanas du för den **API Post URL** och **API-nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-141">When the web app first runs it will ask you for the **API Post URL** and **API Key**.</span></span>
   <span data-ttu-id="f98b0-142">Ange de värden som du sparade tidigare (**Begärd URI** och **API-nyckel**respektive).</span><span class="sxs-lookup"><span data-stu-id="f98b0-142">Enter the values you saved earlier (**Request URI** and **API Key**, respectively).</span></span>
     
     <span data-ttu-id="f98b0-143">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-143">Click **Submit**.</span></span>
     
     ![Ange Post URI och API-nyckel][image6]

6. <span data-ttu-id="f98b0-145">Web app visar dess **Webbappkonfigurationen** sidan med de aktuella inställningarna för web service.</span><span class="sxs-lookup"><span data-stu-id="f98b0-145">The web app displays its **Web App Configuration** page with the current web service settings.</span></span> <span data-ttu-id="f98b0-146">Här kan du ändra inställningarna som används av webbappen.</span><span class="sxs-lookup"><span data-stu-id="f98b0-146">Here you can make changes to the settings used by the web app.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f98b0-147">Om du ändrar de här inställningarna endast ändras dem för det här webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f98b0-147">Changing the settings here only changes them for this web app.</span></span> <span data-ttu-id="f98b0-148">Standardinställningarna för webbtjänsten ändras inte.</span><span class="sxs-lookup"><span data-stu-id="f98b0-148">It doesn't change the default settings of your web service.</span></span> <span data-ttu-id="f98b0-149">Till exempel om du ändrar den **beskrivning** här ändras inte den beskrivning som visas på instrumentpanelen web service i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="f98b0-149">For example, if you change the **Description** here it doesn't change the description shown on the web service dashboard in Machine Learning Studio.</span></span>
   > 
   > 
   
    <span data-ttu-id="f98b0-150">När du är klar klickar du på **spara ändringar**, och klicka sedan på **gå till startsidan**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-150">When you're done, click **Save changes**, and then click **Go to Home Page**.</span></span>

7. <span data-ttu-id="f98b0-151">Du kan ange värden ska skickas till webbtjänsten från startsidan.</span><span class="sxs-lookup"><span data-stu-id="f98b0-151">From the home page you can enter values to send to your web service.</span></span> <span data-ttu-id="f98b0-152">Klicka på **skicka** när du är klar och resultatet returneras.</span><span class="sxs-lookup"><span data-stu-id="f98b0-152">Click **Submit** when you're done, and the result will be returned.</span></span>

<span data-ttu-id="f98b0-153">Om du vill gå tillbaka till den **Configuration** går du till den `setting.aspx` sidan i webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f98b0-153">If you want to return to the **Configuration** page, go to the `setting.aspx` page of the web app.</span></span> <span data-ttu-id="f98b0-154">Till exempel: `http://carprediction.azurewebsites.net/setting.aspx.` uppmanas du att ange API-nyckeln igen – du behöver som kan komma åt sidan och uppdatera inställningarna.</span><span class="sxs-lookup"><span data-stu-id="f98b0-154">For example: `http://carprediction.azurewebsites.net/setting.aspx.` You will be prompted to enter the API key again - you need that to access the page and update the settings.</span></span>

<span data-ttu-id="f98b0-155">Du kan stoppa, starta om eller ta bort webbprogrammet i Azure-portalen som andra webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f98b0-155">You can stop, restart, or delete the web app in the Azure portal like any other web app.</span></span> <span data-ttu-id="f98b0-156">Så länge som den körs kan du bläddra till hem webbadressen och ange nya värden.</span><span class="sxs-lookup"><span data-stu-id="f98b0-156">As long as it is running you can browse to the home web address and enter new values.</span></span>

## <a name="how-to-use-the-batch-execution-service-bes-template"></a><span data-ttu-id="f98b0-157">Hur du använder Batch Execution Service BES-mall</span><span class="sxs-lookup"><span data-stu-id="f98b0-157">How to use the Batch Execution Service (BES) template</span></span>
<span data-ttu-id="f98b0-158">Du kan använda mallen BES web app på samma sätt som RR-mall, förutom att webbappen har skapats kan du skicka flera rader med data och ta emot flera resultat.</span><span class="sxs-lookup"><span data-stu-id="f98b0-158">You can use the BES web app template in the same way as the RRS template, except that the web app that's created will allow you to submit multiple rows of data and receive multiple results.</span></span>

<span data-ttu-id="f98b0-159">Indatavärden för en webbtjänst för batch-körningen kan komma från Azure-lagring eller en lokal fil. resultatet lagras i en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="f98b0-159">The input values for a batch execution web service can come from Azure storage or a local file; the results are stored in an Azure storage container.</span></span>
<span data-ttu-id="f98b0-160">Så du behöver en Azure storage-behållare för att hålla resultaten som returnerades av webbprogrammet och måste du förbereda dina indata.</span><span class="sxs-lookup"><span data-stu-id="f98b0-160">So, you'll need an Azure storage container to hold the results returned by the web app, and you'll need to get your input data ready.</span></span>

![Processen för att använda mallen för BES-webbtjänst][image2]

1. <span data-ttu-id="f98b0-162">Följ samma procedur för att skapa webbprogram BES som mallen Resursposter utom gå till [Azure ML Batch Execution Service Web Appmallen](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) öppnas BES-mallen på Azure Marketplace och på **skapa webbprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-162">Follow the same procedure to create the BES web app as for the RRS template, except go to [Azure ML Batch Execution Service Web App Template](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) to open the BES template on Azure Marketplace and click **Create Web App**.</span></span>

2. <span data-ttu-id="f98b0-163">Där du vill att resultatet lagras ange mål behållaren information på sidan web app.</span><span class="sxs-lookup"><span data-stu-id="f98b0-163">To specify where you want the results stored, enter the destination container information on the web app home page.</span></span> <span data-ttu-id="f98b0-164">Ange också där webbprogrammet får indatavärden, antingen i en lokal fil eller en Azure storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="f98b0-164">Also specify where the web app can get the input values, either in a local file or an Azure storage container.</span></span>
   <span data-ttu-id="f98b0-165">Klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="f98b0-165">Click **Submit**.</span></span>
   
    ![Storage-informationen][image7]

<span data-ttu-id="f98b0-167">Webbprogrammet visas en sida med jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="f98b0-167">The web app will display a page with job status.</span></span>
<span data-ttu-id="f98b0-168">När jobbet har slutförts får du platsen för resultaten i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="f98b0-168">When the job has completed you'll be given the location of the results in Azure blob storage.</span></span> <span data-ttu-id="f98b0-169">Du har också möjlighet att hämta resultaten till en lokal fil.</span><span class="sxs-lookup"><span data-stu-id="f98b0-169">You also have the option of downloading the results to a local file.</span></span>

## <a name="for-more-information"></a><span data-ttu-id="f98b0-170">Mer information</span><span class="sxs-lookup"><span data-stu-id="f98b0-170">For more information</span></span>
<span data-ttu-id="f98b0-171">Mer information om...</span><span class="sxs-lookup"><span data-stu-id="f98b0-171">To learn more about...</span></span>

* <span data-ttu-id="f98b0-172">Skapa ett experiment i machine learning med Machine Learning Studio finns [skapa ditt första experiment i Azure Machine Learning Studio](machine-learning-create-experiment.md)</span><span class="sxs-lookup"><span data-stu-id="f98b0-172">creating a machine learning experiment with Machine Learning Studio, see [Create your first experiment in Azure Machine Learning Studio](machine-learning-create-experiment.md)</span></span>
* <span data-ttu-id="f98b0-173">hur du distribuerar ditt machine learning-experiment som en webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md)</span><span class="sxs-lookup"><span data-stu-id="f98b0-173">how to deploy your machine learning experiment as a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md)</span></span>
* <span data-ttu-id="f98b0-174">andra sätt att få åtkomst till din webbtjänsten finns [använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md)</span><span class="sxs-lookup"><span data-stu-id="f98b0-174">other ways to access your web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md)</span></span>

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
