---
title: "aaaConnect tooa Machine Learning-webbtjänst | Microsoft Docs"
description: "Med C# eller Python, ansluta tooan Azure Machine Learning webbtjänst med hjälp av auktoriseringsnyckel."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 59b07bab-b60f-48c4-a385-a162e50ec7c2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: garye
ROBOTS: NOINDEX
redirect_url: machine-learning-consume-web-services
redirect_document_id: True
ms.openlocfilehash: 0108e71e30a05539a8c0ee93d5aadb07e3d1efa9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-azure-machine-learning-web-service"></a><span data-ttu-id="6aedf-103">Ansluta tooan Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6aedf-103">Connect tooan Azure Machine Learning Web Service</span></span>
<span data-ttu-id="6aedf-104">hello Azure Machine Learning developer upplevelse är en Web service API toomake förutsägelser från indata i realtid eller i batch-läge.</span><span class="sxs-lookup"><span data-stu-id="6aedf-104">hello Azure Machine Learning developer experience is a Web service API toomake predictions from input data in real time or in batch mode.</span></span> <span data-ttu-id="6aedf-105">Du använder Azure Machine Learning Studio toocreate förutsägelser och distribuera en Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-105">You use Azure Machine Learning Studio toocreate predictions and deploy an Azure Machine Learning Web service.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<span data-ttu-id="6aedf-106">toolearn om hur toocreate och distribuera en Machine Learning-webbtjänst med hjälp av Machine Learning Studio:</span><span class="sxs-lookup"><span data-stu-id="6aedf-106">toolearn about how toocreate and deploy a Machine Learning Web service using Machine Learning Studio:</span></span>

* <span data-ttu-id="6aedf-107">En självstudiekurs om hur toocreate ett experiment i Machine Learning Studio finns [skapa ditt första experiment](machine-learning-create-experiment.md).</span><span class="sxs-lookup"><span data-stu-id="6aedf-107">For a tutorial on how toocreate an experiment in Machine Learning Studio, see [Create your first experiment](machine-learning-create-experiment.md).</span></span>
* <span data-ttu-id="6aedf-108">Mer information om hur toodeploy en webbtjänst finns [distribuera en Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6aedf-108">For details on how toodeploy a Web service, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="6aedf-109">Mer information om Machine Learning finns på hello [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span><span class="sxs-lookup"><span data-stu-id="6aedf-109">For more information about Machine Learning in general, visit hello [Machine Learning Documentation Center](https://azure.microsoft.com/documentation/services/machine-learning/).</span></span>

## <a name="azure-machine-learning-web-service"></a><span data-ttu-id="6aedf-110">Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6aedf-110">Azure Machine Learning Web service</span></span>
<span data-ttu-id="6aedf-111">Ett externt program kommunicerar med en Machine Learning arbetsflöde bedömningsprofil modell i realtid med hello Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-111">With hello Azure Machine Learning Web service, an external application communicates with a Machine Learning workflow scoring model in real time.</span></span> <span data-ttu-id="6aedf-112">Ett Machine Learning webbtjänstanrop returnerar resultat för förutsägelse tooan externt program.</span><span class="sxs-lookup"><span data-stu-id="6aedf-112">A Machine Learning Web service call returns prediction results tooan external application.</span></span> <span data-ttu-id="6aedf-113">toomake ett Machine Learning webbtjänstanrop du skickar en API-nyckel som skapas när du distribuerar en förutsägelse.</span><span class="sxs-lookup"><span data-stu-id="6aedf-113">toomake a Machine Learning Web service call, you pass an API key that is created when you deploy a prediction.</span></span> <span data-ttu-id="6aedf-114">hello Machine Learning-webbtjänst baseras på REST, en populär arkitekturval för webb-programmering projekt.</span><span class="sxs-lookup"><span data-stu-id="6aedf-114">hello Machine Learning Web service is based on REST, a popular architecture choice for web programming projects.</span></span>

<span data-ttu-id="6aedf-115">Azure Machine Learning har två typer av tjänster:</span><span class="sxs-lookup"><span data-stu-id="6aedf-115">Azure Machine Learning has two types of services:</span></span>

* <span data-ttu-id="6aedf-116">Svar på begäranden tjänsten (RR) – en låg latens, mycket skalbar tjänst som tillhandahåller ett gränssnitt toohello tillståndslösa modeller skapas och distribueras från hello Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="6aedf-116">Request-Response Service (RRS) – A low latency, highly scalable service that provides an interface toohello stateless models created and deployed from hello Machine Learning Studio.</span></span>
* <span data-ttu-id="6aedf-117">Batch Execution Service (BES) – en asynkron tjänsten som resultat en batch för dataposter.</span><span class="sxs-lookup"><span data-stu-id="6aedf-117">Batch Execution Service (BES) – An asynchronous service that scores a batch for data records.</span></span>

<span data-ttu-id="6aedf-118">Mer information om Machine Learning-webbtjänster finns [distribuera en Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6aedf-118">For more information about Machine Learning Web services, see [Deploy a Machine Learning Web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

## <a name="get-an-azure-machine-learning-authorization-key"></a><span data-ttu-id="6aedf-119">Hämta auktoriseringsnyckel Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="6aedf-119">Get an Azure Machine Learning authorization key</span></span>
<span data-ttu-id="6aedf-120">När du distribuerar experimentet genereras API-nycklar för hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="6aedf-120">When you deploy your experiment, API keys are generated for hello Web service.</span></span> <span data-ttu-id="6aedf-121">Du kan hämta hello nycklar från flera platser.</span><span class="sxs-lookup"><span data-stu-id="6aedf-121">You can retrieve hello keys from several locations.</span></span>

### <a name="from-hello-microsoft-azure-machine-learning-web-services-portal"></a><span data-ttu-id="6aedf-122">Från hello-portalen för Microsoft Azure Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="6aedf-122">From hello Microsoft Azure Machine Learning Web Services portal</span></span>
<span data-ttu-id="6aedf-123">Logga in toohello [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net) portal.</span><span class="sxs-lookup"><span data-stu-id="6aedf-123">Sign in toohello [Microsoft Azure Machine Learning Web Services](https://services.azureml.net) portal.</span></span>

<span data-ttu-id="6aedf-124">tooretrieve hello API-nyckel för en ny Machine Learning-webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="6aedf-124">tooretrieve hello API key for a New Machine Learning Web service:</span></span>

1. <span data-ttu-id="6aedf-125">I hello Azure Machine Learning-webbtjänster portalen klickar du på **Web Services** hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="6aedf-125">In hello Azure Machine Learning Web Services portal, click **Web Services** hello top menu.</span></span>
2. <span data-ttu-id="6aedf-126">Klicka på hello-webbtjänst som du vill tooretrieve hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="6aedf-126">Click hello Web service for which you want tooretrieve hello key.</span></span>
3. <span data-ttu-id="6aedf-127">På hello översta menyn **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-127">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="6aedf-128">Kopiera och spara hello **primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-128">Copy and save hello **Primary Key**.</span></span>

<span data-ttu-id="6aedf-129">tooretrieve hello API-nyckel för en klassiska Machine Learning-webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="6aedf-129">tooretrieve hello API key for a Classic Machine Learning Web service:</span></span>

1. <span data-ttu-id="6aedf-130">I hello Azure Machine Learning-webbtjänster portalen klickar du på **klassiska webbtjänster** hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="6aedf-130">In hello Azure Machine Learning Web Services portal, click **Classic Web Services** hello top menu.</span></span>
2. <span data-ttu-id="6aedf-131">Klicka på hello-webbtjänst som du arbetar.</span><span class="sxs-lookup"><span data-stu-id="6aedf-131">Click hello Web service with which you are working.</span></span>
3. <span data-ttu-id="6aedf-132">Klicka på hello-slutpunkt som du vill tooretrieve hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="6aedf-132">Click hello endpoint for which you want tooretrieve hello key.</span></span>
4. <span data-ttu-id="6aedf-133">På hello översta menyn **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-133">On hello top menu, click **Consume**.</span></span>
5. <span data-ttu-id="6aedf-134">Kopiera och spara hello **primärnyckel**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-134">Copy and save hello **Primary Key**.</span></span>

### <a name="classic-web-service"></a><span data-ttu-id="6aedf-135">Klassiska webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6aedf-135">Classic Web service</span></span>
 <span data-ttu-id="6aedf-136">Du kan också hämta en nyckel för det klassiska från Machine Learning Studio eller hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6aedf-136">You can also retrieve a key for a Classic Web service from Machine Learning Studio or hello Azure classic portal.</span></span>

#### <a name="machine-learning-studio"></a><span data-ttu-id="6aedf-137">Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="6aedf-137">Machine Learning Studio</span></span>
1. <span data-ttu-id="6aedf-138">I Machine Learning Studio klickar du på **WEB SERVICES** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="6aedf-138">In Machine Learning Studio, click **WEB SERVICES** on hello left.</span></span>
2. <span data-ttu-id="6aedf-139">Klicka på en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-139">Click a Web service.</span></span> <span data-ttu-id="6aedf-140">Hej **API-nyckel** på hello **INSTRUMENTPANELEN** fliken.</span><span class="sxs-lookup"><span data-stu-id="6aedf-140">hello **API key** is on hello **DASHBOARD** tab.</span></span>

#### <a name="azure-classic-portal"></a><span data-ttu-id="6aedf-141">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="6aedf-141">Azure classic portal</span></span>
1. <span data-ttu-id="6aedf-142">Klicka på **MASKININLÄRNING** hello vänster.</span><span class="sxs-lookup"><span data-stu-id="6aedf-142">Click **MACHINE LEARNING** on hello left.</span></span>
2. <span data-ttu-id="6aedf-143">Klicka på hello arbetsyta där webbtjänsten finns.</span><span class="sxs-lookup"><span data-stu-id="6aedf-143">Click hello workspace in which your Web service is located.</span></span>
3. <span data-ttu-id="6aedf-144">Klicka på **WEBBTJÄNSTER**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-144">Click **WEB SERVICES**.</span></span>
4. <span data-ttu-id="6aedf-145">Klicka på en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-145">Click a Web service.</span></span>
5. <span data-ttu-id="6aedf-146">Klicka på en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6aedf-146">Click an endpoint.</span></span> <span data-ttu-id="6aedf-147">Hej ”API-NYCKELN” är inte tillgänglig på hello längst ned till höger.</span><span class="sxs-lookup"><span data-stu-id="6aedf-147">hello “API KEY” is down at hello lower-right.</span></span>

## <span data-ttu-id="6aedf-148"><a id="connect"></a>Ansluta tooa på Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6aedf-148"><a id="connect"></a>Connect tooa Machine Learning Web service</span></span>
<span data-ttu-id="6aedf-149">Du kan ansluta tooa Machine Learning webbtjänst med hjälp av programmeringsspråk som stöder HTTP-begäran och svar.</span><span class="sxs-lookup"><span data-stu-id="6aedf-149">You can connect tooa Machine Learning Web service using any programming language that supports HTTP request and response.</span></span> <span data-ttu-id="6aedf-150">Du kan visa exempel i C#, Python och R från en Machine Learning-webbtjänstens hjälpsida.</span><span class="sxs-lookup"><span data-stu-id="6aedf-150">You can view examples in C#, Python, and R from a Machine Learning Web service help page.</span></span>

<span data-ttu-id="6aedf-151">**Datorn Learning API hjälp** hjälp med Machine Learning API skapas när du distribuerar en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-151">**Machine Learning API help** Machine Learning API help is created when you deploy a Web service.</span></span> <span data-ttu-id="6aedf-152">Se [Azure Machine Learning genomgången distribuerar webbtjänsten](machine-learning-walkthrough-5-publish-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6aedf-152">See [Azure Machine Learning Walkthrough- Deploy Web Service](machine-learning-walkthrough-5-publish-web-service.md).</span></span>
<span data-ttu-id="6aedf-153">hello Machine Learning API-hjälpen innehåller information om en förutsägelse webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="6aedf-153">hello Machine Learning API help contains details about a prediction Web service.</span></span>

1. <span data-ttu-id="6aedf-154">Klicka på hello-webbtjänst som du arbetar.</span><span class="sxs-lookup"><span data-stu-id="6aedf-154">Click hello Web service with which you are working.</span></span>
2. <span data-ttu-id="6aedf-155">Klicka på hello-slutpunkt som du vill tooview hello API-hjälpsidan.</span><span class="sxs-lookup"><span data-stu-id="6aedf-155">Click hello endpoint for which you want tooview hello API Help Page.</span></span>
3. <span data-ttu-id="6aedf-156">På hello översta menyn **förbruka**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-156">On hello top menu, click **Consume**.</span></span>
4. <span data-ttu-id="6aedf-157">Klicka på **API hjälpsidan** under hello svar på begäranden eller Batch Execution slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="6aedf-157">Click **API help page** under either hello Request-Response or Batch Execution endpoints.</span></span>

<span data-ttu-id="6aedf-158">**tooview Machine Learning API hjälp för en ny webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="6aedf-158">**tooview Machine Learning API help for a New Web service**</span></span>

<span data-ttu-id="6aedf-159">I hello Azure Machine Learning Web Services-portalen:</span><span class="sxs-lookup"><span data-stu-id="6aedf-159">In hello Azure Machine Learning Web Services Portal:</span></span>

1. <span data-ttu-id="6aedf-160">Klicka på **WEB SERVICES** på hello översta menyn.</span><span class="sxs-lookup"><span data-stu-id="6aedf-160">Click **WEB SERVICES** on hello top menu.</span></span>
2. <span data-ttu-id="6aedf-161">Klicka på hello-webbtjänst som du vill tooretrieve hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="6aedf-161">Click hello Web service for which you want tooretrieve hello key.</span></span>

<span data-ttu-id="6aedf-162">Klicka på **förbruka** tooget hello URI: er för hello begäran Reposonse och Batch Execution tjänster och exempel kod i C#, R och Python.</span><span class="sxs-lookup"><span data-stu-id="6aedf-162">Click **Consume** tooget hello URIs for hello Request-Reposonse and Batch Execution Services and Sample code in C#, R, and Python.</span></span>

<span data-ttu-id="6aedf-163">Klicka på **Swagger API** tooget Swagger baserat dokumentationen för hello API: er anropas från hello angivna URI: er.</span><span class="sxs-lookup"><span data-stu-id="6aedf-163">Click **Swagger API** tooget Swagger based documentation for hello APIs called from hello supplied URIs.</span></span>

### <a name="c-sample"></a><span data-ttu-id="6aedf-164">C#-exempel</span><span class="sxs-lookup"><span data-stu-id="6aedf-164">C# Sample</span></span>
<span data-ttu-id="6aedf-165">tooconnect tooa Machine Learning-webbtjänsten, använda en **HttpClient** skicka ScoreData.</span><span class="sxs-lookup"><span data-stu-id="6aedf-165">tooconnect tooa Machine Learning Web service, use an **HttpClient** passing ScoreData.</span></span> <span data-ttu-id="6aedf-166">ScoreData innehåller en FeatureVector en endimensionell n vector av numeriska funktioner som representerar hello ScoreData.</span><span class="sxs-lookup"><span data-stu-id="6aedf-166">ScoreData contains a FeatureVector, an n-dimensional vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="6aedf-167">Du kan autentisera toohello Machine Learning-tjänsten med en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="6aedf-167">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="6aedf-168">tooconnect tooa Machine Learning-webbtjänst, hello **Microsoft.AspNet.WebApi.Client** NuGet-paketet måste vara installerad.</span><span class="sxs-lookup"><span data-stu-id="6aedf-168">tooconnect tooa Machine Learning Web service, hello **Microsoft.AspNet.WebApi.Client** NuGet package must be installed.</span></span>

<span data-ttu-id="6aedf-169">**Installera Microsoft.AspNet.WebApi.Client NuGet i Visual Studio**</span><span class="sxs-lookup"><span data-stu-id="6aedf-169">**Install Microsoft.AspNet.WebApi.Client NuGet in Visual Studio**</span></span>

1. <span data-ttu-id="6aedf-170">Publicera hello Download datauppsättning från UCI: vuxna 2 klass dataset-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-170">Publish hello Download dataset from UCI: Adult 2 class dataset Web Service.</span></span>
2. <span data-ttu-id="6aedf-171">Klicka på **Verktyg** > **NuGet Package Manager** > **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-171">Click **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>
3. <span data-ttu-id="6aedf-172">Välj **Install-Package Microsoft.AspNet.WebApi.Client**.</span><span class="sxs-lookup"><span data-stu-id="6aedf-172">Choose **Install-Package Microsoft.AspNet.WebApi.Client**.</span></span>

<span data-ttu-id="6aedf-173">**toorun hello-kodexempel**</span><span class="sxs-lookup"><span data-stu-id="6aedf-173">**toorun hello code sample**</span></span>

1. <span data-ttu-id="6aedf-174">Publicera ”exempel 1: hämta dataset från UCI: vuxen 2 klass dataset” experiment, en del av hello Machine Learning provtagning.</span><span class="sxs-lookup"><span data-stu-id="6aedf-174">Publish "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="6aedf-175">Tilldela apiKey med hello nyckel från en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-175">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="6aedf-176">Se **hämta auktoriseringsnyckel Azure Machine Learning** ovan.</span><span class="sxs-lookup"><span data-stu-id="6aedf-176">See **Get an Azure Machine Learning authorization key** above.</span></span>
3. <span data-ttu-id="6aedf-177">Tilldela serviceUri med hello Begärd URI.</span><span class="sxs-lookup"><span data-stu-id="6aedf-177">Assign serviceUri with hello Request URI.</span></span>

### <a name="python-sample"></a><span data-ttu-id="6aedf-178">Python-exempel</span><span class="sxs-lookup"><span data-stu-id="6aedf-178">Python Sample</span></span>
<span data-ttu-id="6aedf-179">tooconnect tooa Machine Learning-webbtjänsten, använda hello **urllib2** biblioteket skicka ScoreData.</span><span class="sxs-lookup"><span data-stu-id="6aedf-179">tooconnect tooa Machine Learning Web service, use hello **urllib2** library passing ScoreData.</span></span> <span data-ttu-id="6aedf-180">ScoreData innehåller en FeatureVector en endimensionell n vector av numeriska funktioner som representerar hello ScoreData.</span><span class="sxs-lookup"><span data-stu-id="6aedf-180">ScoreData contains a FeatureVector, an n-dimensional  vector of numerical features that represents hello ScoreData.</span></span> <span data-ttu-id="6aedf-181">Du kan autentisera toohello Machine Learning-tjänsten med en API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="6aedf-181">You authenticate toohello Machine Learning service with an API key.</span></span>

<span data-ttu-id="6aedf-182">**toorun hello-kodexempel**</span><span class="sxs-lookup"><span data-stu-id="6aedf-182">**toorun hello code sample**</span></span>

1. <span data-ttu-id="6aedf-183">Distribuera ”exempel 1: hämta dataset från UCI: vuxen 2 klass dataset” experiment, en del av hello Machine Learning provtagning.</span><span class="sxs-lookup"><span data-stu-id="6aedf-183">Deploy "Sample 1: Download dataset from UCI: Adult 2 class dataset" experiment, part of hello Machine Learning sample collection.</span></span>
2. <span data-ttu-id="6aedf-184">Tilldela apiKey med hello nyckel från en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="6aedf-184">Assign apiKey with hello key from a Web service.</span></span> <span data-ttu-id="6aedf-185">Se hello **hämta auktoriseringsnyckel Azure Machine Learning** avsnittet hello början av den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6aedf-185">See hello **Get an Azure Machine Learning authorization key** section near hello beginning of this article.</span></span>
3. <span data-ttu-id="6aedf-186">Tilldela serviceUri med hello Begärd URI.</span><span class="sxs-lookup"><span data-stu-id="6aedf-186">Assign serviceUri with hello Request URI.</span></span>

