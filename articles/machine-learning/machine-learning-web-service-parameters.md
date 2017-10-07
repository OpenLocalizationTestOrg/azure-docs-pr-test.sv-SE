---
title: "aaaUse Webbtjänstparametrar som Azure Machine Learning | Microsoft Docs"
description: "Hur hello toouse Azure Machine Learning Webbtjänstparametrar toomodify beteendet för din modell när hello webbtjänsten används."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 214711eb819a6cea34db905abdf015da11e846d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a><span data-ttu-id="82046-103">Använda parametrar för Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="82046-103">Use Azure Machine Learning Web Service Parameters</span></span>
<span data-ttu-id="82046-104">En Azure Machine Learning-webbtjänst skapas genom att publicera ett experiment som innehåller moduler med parametrar.</span><span class="sxs-lookup"><span data-stu-id="82046-104">An Azure Machine Learning web service is created by publishing an experiment that contains modules with configurable parameters.</span></span> <span data-ttu-id="82046-105">I vissa fall kan kanske du vill toochange hello modulen beteende när hello-webbtjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="82046-105">In some cases, you may want toochange hello module behavior while hello web service is running.</span></span> <span data-ttu-id="82046-106">*Webbtjänstparametrar* Tillåt toodo den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="82046-106">*Web Service Parameters* allow you toodo this task.</span></span> 

<span data-ttu-id="82046-107">Ett vanligt exempel att konfigurera hello [importera Data] [ reader] modulen så att användaren hello hello publicerade webbtjänsten kan ange en annan datakälla när hello webbtjänsten används.</span><span class="sxs-lookup"><span data-stu-id="82046-107">A common example is setting up hello [Import Data][reader] module so that hello user of hello published web service can specify a different data source when hello web service is accessed.</span></span> <span data-ttu-id="82046-108">Eller konfigurera hello [exportera Data] [ writer] modulen så att du kan ange ett annat mål.</span><span class="sxs-lookup"><span data-stu-id="82046-108">Or configuring hello [Export Data][writer] module so that a different destination can be specified.</span></span> <span data-ttu-id="82046-109">Andra exempel är att ändra hello antalet bitar för hello [funktions-hashning] [ feature-hashing] modul eller hello antalet önskade funktioner för hello [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modul.</span><span class="sxs-lookup"><span data-stu-id="82046-109">Some other examples include changing hello number of bits for hello [Feature Hashing][feature-hashing] module or hello number of desired features for hello [Filter-Based Feature Selection][filter-based-feature-selection] module.</span></span> 

<span data-ttu-id="82046-110">Du kan ange Webbtjänstparametrar och koppla dem till en eller flera parametrar i experimentet och du kan ange om de är obligatoriska eller valfria.</span><span class="sxs-lookup"><span data-stu-id="82046-110">You can set Web Service Parameters and associate them with one or more module parameters in your experiment, and you can specify whether they are required or optional.</span></span> <span data-ttu-id="82046-111">hello användaren av webbtjänsten för hello kan sedan ange värden för dessa parametrar när de anropa hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="82046-111">hello user of hello web service can then provide values for these parameters when they call hello web service.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-tooset-and-use-web-service-parameters"></a><span data-ttu-id="82046-112">Hur tooset och använda Webbtjänstparametrar</span><span class="sxs-lookup"><span data-stu-id="82046-112">How tooset and use Web Service Parameters</span></span>
<span data-ttu-id="82046-113">Du kan definiera en Web Service-Parameter klickar hello ikonen nästa toohello parameter för en modul och väljer ”Ange som web service parameter”.</span><span class="sxs-lookup"><span data-stu-id="82046-113">You define a Web Service Parameter by clicking hello icon next toohello parameter for a module and selecting "Set as web service parameter".</span></span> <span data-ttu-id="82046-114">Detta skapar en ny Web Service Parameter och kopplar den toothat modulen parametern.</span><span class="sxs-lookup"><span data-stu-id="82046-114">This creates a new Web Service Parameter and connects it toothat module parameter.</span></span> <span data-ttu-id="82046-115">Sedan när hello webbtjänsten används hello användaren kan ange ett värde för hello Web Service-parametern och det är tillämpade toohello modulen parameter.</span><span class="sxs-lookup"><span data-stu-id="82046-115">Then, when hello web service is accessed, hello user can specify a value for hello Web Service Parameter and it is applied toohello module parameter.</span></span>

<span data-ttu-id="82046-116">När du definierar en Web Service-Parameter, är det tillgängliga tooany andra modul-parametern i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="82046-116">Once you define a Web Service Parameter, it's available tooany other module parameter in hello experiment.</span></span> <span data-ttu-id="82046-117">Om du definierar en Web Service-Parameter som är associerade med en parameter för en modul, kan du använda samma Web Service parametern för en annan modul som hello parametern förväntar hello samma typ av värde.</span><span class="sxs-lookup"><span data-stu-id="82046-117">If you define a Web Service Parameter associated with a parameter for one module, you can use that same Web Service Parameter for any other module, as long as hello parameter expects hello same type of value.</span></span> <span data-ttu-id="82046-118">Till exempel om hello Web Service-parametern är ett numeriskt värde, kan sedan den bara användas för parametrar som förväntar sig ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="82046-118">For example, if hello Web Service Parameter is a numeric value, then it can only be used for module parameters that expect a numeric value.</span></span> <span data-ttu-id="82046-119">Hello användare anger ett värde för hello Web Service-Parameter, blir tillämpade tooall associerade parametrar.</span><span class="sxs-lookup"><span data-stu-id="82046-119">When hello user sets a value for hello Web Service Parameter, it will be applied tooall associated module parameters.</span></span>

<span data-ttu-id="82046-120">Du kan bestämma om tooprovide standard värde för hello Web Service-parametern.</span><span class="sxs-lookup"><span data-stu-id="82046-120">You can decide whether tooprovide a default value for hello Web Service Parameter.</span></span> <span data-ttu-id="82046-121">Om du gör sedan hello är parametern valfri för hello användare av hello web service.</span><span class="sxs-lookup"><span data-stu-id="82046-121">If you do, then hello parameter is optional for hello user of hello web service.</span></span> <span data-ttu-id="82046-122">Om du inte anger ett standardvärde kan sedan hello användaren är obligatoriska tooenter ett värde när hello webbtjänsten används.</span><span class="sxs-lookup"><span data-stu-id="82046-122">If you don't provide a default value, then hello user is required tooenter a value when hello web service is accessed.</span></span>

<span data-ttu-id="82046-123">hello API-dokumentationen för hello-webbtjänsten innehåller information för hello web Serviceanvändare på hur toospecify hello Web Service parametern programmässigt vid åtkomst till hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="82046-123">hello API documentation for hello web service includes information for hello web service user on how toospecify hello Web Service Parameter programmatically when accessing hello web service.</span></span>

> [!NOTE]
> <span data-ttu-id="82046-124">Hej API-dokumentationen för en klassiska webbtjänst tillhandahålls via hello **API hjälpsidan** länken i hello webbtjänsten **INSTRUMENTPANELEN** i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="82046-124">hello API documentation for a classic web service is provided through hello **API help page** link in hello web service **DASHBOARD** in Machine Learning Studio.</span></span> <span data-ttu-id="82046-125">Hej API-dokumentationen för en ny webbtjänst tillhandahålls via hello [Azure Machine Learning-webbtjänster](https://services.azureml.net/Quickstart) Företagsportalen på hello **förbruka** och **Swagger API** sidor för din webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="82046-125">hello API documentation for a new web service is provided through hello [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) portal on hello **Consume** and **Swagger API** pages for your web service.</span></span>
> 
> 

## <a name="example"></a><span data-ttu-id="82046-126">Exempel</span><span class="sxs-lookup"><span data-stu-id="82046-126">Example</span></span>
<span data-ttu-id="82046-127">Ett exempel antar vi att vi har ett experiment med en [exportera Data] [ writer] modul som skickar information tooAzure blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="82046-127">As an example, let's assume we have an experiment with an [Export Data][writer] module that sends information tooAzure blob storage.</span></span> <span data-ttu-id="82046-128">Ska vi definiera Web Service Parameter med namnet ”Blob sökvägen” som gör att hello web service användaren toochange hello sökvägen toohello blob-lagring när hello tjänsten används.</span><span class="sxs-lookup"><span data-stu-id="82046-128">We'll define a Web Service Parameter named "Blob path" that allows hello web service user toochange hello path toohello blob storage when hello service is accessed.</span></span>

1. <span data-ttu-id="82046-129">Klicka på hello i Machine Learning Studio [exportera Data] [ writer] modulen tooselect den.</span><span class="sxs-lookup"><span data-stu-id="82046-129">In Machine Learning Studio, click hello [Export Data][writer] module tooselect it.</span></span> <span data-ttu-id="82046-130">Egenskaperna som visas i hello egenskaper fönstret toohello höger i hello experimentet.</span><span class="sxs-lookup"><span data-stu-id="82046-130">Its properties are shown in hello Properties pane toohello right of hello experiment canvas.</span></span>
2. <span data-ttu-id="82046-131">Ange hello lagringstyp:</span><span class="sxs-lookup"><span data-stu-id="82046-131">Specify hello storage type:</span></span>
   
   * <span data-ttu-id="82046-132">Under **ange datamål**, Välj ”Azure Blob Storage”.</span><span class="sxs-lookup"><span data-stu-id="82046-132">Under **Please specify data destination**, select "Azure Blob Storage".</span></span>
   * <span data-ttu-id="82046-133">Under **ange autentiseringstypen**, Välj ”konto”.</span><span class="sxs-lookup"><span data-stu-id="82046-133">Under **Please specify authentication type**, select "Account".</span></span>
   * <span data-ttu-id="82046-134">Ange hello kontoinformationen för hello Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="82046-134">Enter hello account information for hello Azure blob storage.</span></span> 
     <p /><span data-ttu-id="82046-135">
3.Klicka på hello ikonen toohello höger i hello **sökväg tooblob som inleds med behållaren parametern**.</span><span class="sxs-lookup"><span data-stu-id="82046-135">
3. Click hello icon toohello right of hello **Path tooblob beginning with container parameter**.</span></span> <span data-ttu-id="82046-136">Det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="82046-136">It looks like this:</span></span>
   
   ![Web Service-parametern ikonen][icon]
   
   <span data-ttu-id="82046-138">Välj ”ange som web service parameter”.</span><span class="sxs-lookup"><span data-stu-id="82046-138">Select "Set as web service parameter".</span></span>
   
   <span data-ttu-id="82046-139">En post läggs till under **Webbtjänstparametrar** längst hello hello egenskapsrutan med hello namn ”sökväg tooblob som börjar med behållaren”.</span><span class="sxs-lookup"><span data-stu-id="82046-139">An entry is added under **Web Service Parameters** at hello bottom of hello Properties pane with hello name "Path tooblob beginning with container".</span></span> <span data-ttu-id="82046-140">Detta är hello Web Service-Parameter som är nu som är associerade med den här [exportera Data] [ writer] modul-parametern.</span><span class="sxs-lookup"><span data-stu-id="82046-140">This is hello Web Service Parameter that is now associated with this [Export Data][writer] module parameter.</span></span>
4. <span data-ttu-id="82046-141">toorename hello Web Service-parametern, hello klickar du på, ange ”Blob sökvägen” och tryck på hello **RETUR** nyckel.</span><span class="sxs-lookup"><span data-stu-id="82046-141">toorename hello Web Service Parameter, click hello name, enter "Blob path", and press hello **Enter** key.</span></span> 
5. <span data-ttu-id="82046-142">tooprovide ett standardvärde för hello Web Service-Parameter, klicka på hello ikonen toohello höger i hello namn, markera ”ge standardvärdet”, ange ett värde (till exempel ”container1/output1.csv”) och tryck på hello **RETUR** nyckel.</span><span class="sxs-lookup"><span data-stu-id="82046-142">tooprovide a default value for hello Web Service Parameter, click hello icon toohello right of hello name, select "Provide default value", enter a value (for example, "container1/output1.csv"), and press hello **Enter** key.</span></span>
   
   ![Web Service-Parameter][parameter]
6. <span data-ttu-id="82046-144">Klicka på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="82046-144">Click **Run**.</span></span> 
7. <span data-ttu-id="82046-145">Klicka på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]** toodeploy hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="82046-145">Click **Deploy Web Service** and select **Deploy Web Service [Classic]** or **Deploy Web Service [New]** toodeploy hello web service.</span></span>

> [!NOTE] 
> <span data-ttu-id="82046-146">toodeploy en ny webbtjänst som du måste ha tillräckliga behörigheter i hello prenumeration toowhich du distribuerar hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="82046-146">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="82046-147">Mer information finns i [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="82046-147">For more information see, [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="82046-148">hello användaren av webbtjänsten hello kan nu ange ett nytt mål för hello [exportera Data] [ writer] modulen vid åtkomst till hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="82046-148">hello user of hello web service can now specify a new destination for hello [Export Data][writer] module when accessing hello web service.</span></span>

## <a name="more-information"></a><span data-ttu-id="82046-149">Mer information</span><span class="sxs-lookup"><span data-stu-id="82046-149">More information</span></span>
<span data-ttu-id="82046-150">En mer detaljerad exempel finns hello [Webbtjänstparametrar](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) post i hello [Machine Learning blogg](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span><span class="sxs-lookup"><span data-stu-id="82046-150">For a more detailed example, see hello [Web Service Parameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) entry in hello [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span></span>

<span data-ttu-id="82046-151">Mer information om hur du använder en Machine Learning-webbtjänst finns [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="82046-151">For more information on accessing a Machine Learning web service, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

