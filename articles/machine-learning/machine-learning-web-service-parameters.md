---
title: "Använda Azure Machine Learning Webbtjänstparametrar | Microsoft Docs"
description: "Hur du använder Azure Machine Learning Webbtjänstparametrar för att ändra funktionssättet för din modell vid åtkomst av webbtjänsten."
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
ms.openlocfilehash: 482726c1dae5385964e08b720e529817d5907537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a><span data-ttu-id="06229-103">Använda parametrar för Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="06229-103">Use Azure Machine Learning Web Service Parameters</span></span>
<span data-ttu-id="06229-104">En Azure Machine Learning-webbtjänst skapas genom att publicera ett experiment som innehåller moduler med parametrar.</span><span class="sxs-lookup"><span data-stu-id="06229-104">An Azure Machine Learning web service is created by publishing an experiment that contains modules with configurable parameters.</span></span> <span data-ttu-id="06229-105">I vissa fall kanske du vill ändra modulen beteendet när webbtjänsten körs.</span><span class="sxs-lookup"><span data-stu-id="06229-105">In some cases, you may want to change the module behavior while the web service is running.</span></span> <span data-ttu-id="06229-106">*Webbtjänstparametrar* gör att du kan göra detta.</span><span class="sxs-lookup"><span data-stu-id="06229-106">*Web Service Parameters* allow you to do this task.</span></span> 

<span data-ttu-id="06229-107">Ett vanligt exempel att konfigurera den [importera Data] [ reader] modulen så att användaren av webbtjänsten för publicerade kan ange en annan datakälla när webbtjänsten används.</span><span class="sxs-lookup"><span data-stu-id="06229-107">A common example is setting up the [Import Data][reader] module so that the user of the published web service can specify a different data source when the web service is accessed.</span></span> <span data-ttu-id="06229-108">Eller konfigurera den [exportera Data] [ writer] modulen så att du kan ange ett annat mål.</span><span class="sxs-lookup"><span data-stu-id="06229-108">Or configuring the [Export Data][writer] module so that a different destination can be specified.</span></span> <span data-ttu-id="06229-109">Andra exempel är att ändra antalet bitar för den [funktions-hashning] [ feature-hashing] modul eller antalet önskade funktioner för den [Filter-baserade Funktionsurval] [ filter-based-feature-selection] modul.</span><span class="sxs-lookup"><span data-stu-id="06229-109">Some other examples include changing the number of bits for the [Feature Hashing][feature-hashing] module or the number of desired features for the [Filter-Based Feature Selection][filter-based-feature-selection] module.</span></span> 

<span data-ttu-id="06229-110">Du kan ange Webbtjänstparametrar och koppla dem till en eller flera parametrar i experimentet och du kan ange om de är obligatoriska eller valfria.</span><span class="sxs-lookup"><span data-stu-id="06229-110">You can set Web Service Parameters and associate them with one or more module parameters in your experiment, and you can specify whether they are required or optional.</span></span> <span data-ttu-id="06229-111">Användaren av webbtjänsten kan sedan ge värdena för dessa parametrar när de anropa webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-111">The user of the web service can then provide values for these parameters when they call the web service.</span></span> 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-to-set-and-use-web-service-parameters"></a><span data-ttu-id="06229-112">Ställa in och använda Webbtjänstparametrar</span><span class="sxs-lookup"><span data-stu-id="06229-112">How to set and use Web Service Parameters</span></span>
<span data-ttu-id="06229-113">Du kan definiera en Web Service-Parameter genom att klicka på ikonen bredvid parametern för en modul och välja ”Ange som web service parameter”.</span><span class="sxs-lookup"><span data-stu-id="06229-113">You define a Web Service Parameter by clicking the icon next to the parameter for a module and selecting "Set as web service parameter".</span></span> <span data-ttu-id="06229-114">Detta skapar en ny Parameter i Web Service och ansluter till den modul-parametern.</span><span class="sxs-lookup"><span data-stu-id="06229-114">This creates a new Web Service Parameter and connects it to that module parameter.</span></span> <span data-ttu-id="06229-115">Sedan, när webbtjänsten används användaren kan ange ett värde för parametern Web Service och den tillämpas på parametern modulen.</span><span class="sxs-lookup"><span data-stu-id="06229-115">Then, when the web service is accessed, the user can specify a value for the Web Service Parameter and it is applied to the module parameter.</span></span>

<span data-ttu-id="06229-116">När du definierar en Web Service-Parameter, går det att andra modulen parameter i experimentet.</span><span class="sxs-lookup"><span data-stu-id="06229-116">Once you define a Web Service Parameter, it's available to any other module parameter in the experiment.</span></span> <span data-ttu-id="06229-117">Om du definierar en Web Service-Parameter som är associerade med en parameter för en modul, kan du använda samma Web Service parametern för en annan modul som parametern förväntar sig samma typ av värde.</span><span class="sxs-lookup"><span data-stu-id="06229-117">If you define a Web Service Parameter associated with a parameter for one module, you can use that same Web Service Parameter for any other module, as long as the parameter expects the same type of value.</span></span> <span data-ttu-id="06229-118">Till exempel om parametrarna för webbtjänsten är ett numeriskt värde, kan sedan den bara användas för parametrar som förväntar sig ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="06229-118">For example, if the Web Service Parameter is a numeric value, then it can only be used for module parameters that expect a numeric value.</span></span> <span data-ttu-id="06229-119">När användaren anger ett värde för parametern Web Service, tillämpas den på alla associerade parametrar.</span><span class="sxs-lookup"><span data-stu-id="06229-119">When the user sets a value for the Web Service Parameter, it will be applied to all associated module parameters.</span></span>

<span data-ttu-id="06229-120">Du kan bestämma om du vill ange ett standardvärde för parametrarna för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-120">You can decide whether to provide a default value for the Web Service Parameter.</span></span> <span data-ttu-id="06229-121">Om du gör sedan är parametern valfri för användaren av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-121">If you do, then the parameter is optional for the user of the web service.</span></span> <span data-ttu-id="06229-122">Om du inte anger ett standardvärde krävs användaren att ange ett värde vid åtkomst av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-122">If you don't provide a default value, then the user is required to enter a value when the web service is accessed.</span></span>

<span data-ttu-id="06229-123">API-dokumentationen för webbtjänsten innehåller information för web Serviceanvändare om hur du anger parametrarna för webbtjänsten programmässigt vid åtkomst till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-123">The API documentation for the web service includes information for the web service user on how to specify the Web Service Parameter programmatically when accessing the web service.</span></span>

> [!NOTE]
> <span data-ttu-id="06229-124">API-dokumentationen för det klassiska tillhandahålls via den **API hjälpsidan** länken i webbtjänsten **INSTRUMENTPANELEN** i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="06229-124">The API documentation for a classic web service is provided through the **API help page** link in the web service **DASHBOARD** in Machine Learning Studio.</span></span> <span data-ttu-id="06229-125">API-dokumentationen för en ny webbtjänst tillhandahålls via den [Azure Machine Learning-webbtjänster](https://services.azureml.net/Quickstart) Företagsportalen på den **förbruka** och **Swagger API** sidor för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-125">The API documentation for a new web service is provided through the [Azure Machine Learning Web Services](https://services.azureml.net/Quickstart) portal on the **Consume** and **Swagger API** pages for your web service.</span></span>
> 
> 

## <a name="example"></a><span data-ttu-id="06229-126">Exempel</span><span class="sxs-lookup"><span data-stu-id="06229-126">Example</span></span>
<span data-ttu-id="06229-127">Ett exempel antar vi att vi har ett experiment med en [exportera Data] [ writer] modul som skickar information till Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="06229-127">As an example, let's assume we have an experiment with an [Export Data][writer] module that sends information to Azure blob storage.</span></span> <span data-ttu-id="06229-128">Ska vi definiera Web Service Parameter med namnet ”Blob sökvägen” som gör att web service användaren kan ändra sökvägen till blob-lagring när tjänsten används.</span><span class="sxs-lookup"><span data-stu-id="06229-128">We'll define a Web Service Parameter named "Blob path" that allows the web service user to change the path to the blob storage when the service is accessed.</span></span>

1. <span data-ttu-id="06229-129">I Machine Learning Studio klickar du på den [exportera Data] [ writer] modulen så att den markeras.</span><span class="sxs-lookup"><span data-stu-id="06229-129">In Machine Learning Studio, click the [Export Data][writer] module to select it.</span></span> <span data-ttu-id="06229-130">Egenskaperna som visas i fönstret egenskaper till höger om arbetsytan för experimentet.</span><span class="sxs-lookup"><span data-stu-id="06229-130">Its properties are shown in the Properties pane to the right of the experiment canvas.</span></span>
2. <span data-ttu-id="06229-131">Ange vilken lagringstyp:</span><span class="sxs-lookup"><span data-stu-id="06229-131">Specify the storage type:</span></span>
   
   * <span data-ttu-id="06229-132">Under **ange datamål**, Välj ”Azure Blob Storage”.</span><span class="sxs-lookup"><span data-stu-id="06229-132">Under **Please specify data destination**, select "Azure Blob Storage".</span></span>
   * <span data-ttu-id="06229-133">Under **ange autentiseringstypen**, Välj ”konto”.</span><span class="sxs-lookup"><span data-stu-id="06229-133">Under **Please specify authentication type**, select "Account".</span></span>
   * <span data-ttu-id="06229-134">Ange kontoinformationen för Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="06229-134">Enter the account information for the Azure blob storage.</span></span> 
     <p /><span data-ttu-id="06229-135">
3.Klicka på ikonen till höger om den **sökväg till blob som börjar med behållaren parametern**.</span><span class="sxs-lookup"><span data-stu-id="06229-135">
3. Click the icon to the right of the **Path to blob beginning with container parameter**.</span></span> <span data-ttu-id="06229-136">Det ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="06229-136">It looks like this:</span></span>
   
   ![Web Service-parametern ikonen][icon]
   
   <span data-ttu-id="06229-138">Välj ”ange som web service parameter”.</span><span class="sxs-lookup"><span data-stu-id="06229-138">Select "Set as web service parameter".</span></span>
   
   <span data-ttu-id="06229-139">En post läggs till under **Webbtjänstparametrar** längst ned i fönstret Egenskaper med namnet ”sökväg till blob som börjar med behållaren”.</span><span class="sxs-lookup"><span data-stu-id="06229-139">An entry is added under **Web Service Parameters** at the bottom of the Properties pane with the name "Path to blob beginning with container".</span></span> <span data-ttu-id="06229-140">Detta är parametrarna för webbtjänsten som nu är associerade med den här [exportera Data] [ writer] modulen parametern.</span><span class="sxs-lookup"><span data-stu-id="06229-140">This is the Web Service Parameter that is now associated with this [Export Data][writer] module parameter.</span></span>
4. <span data-ttu-id="06229-141">Att byta namn på parametrarna för webbtjänsten, klickar du på namnet, anger du ”blobbsökvägen”, och tryck på den **RETUR** nyckel.</span><span class="sxs-lookup"><span data-stu-id="06229-141">To rename the Web Service Parameter, click the name, enter "Blob path", and press the **Enter** key.</span></span> 
5. <span data-ttu-id="06229-142">Om du vill ange ett standardvärde för parametern Web Service, klickar du på ikonen till höger om namnet väljer ”ge standardvärdet”, ange ett värde (till exempel ”container1/output1.csv”), och tryck på den **RETUR** nyckel.</span><span class="sxs-lookup"><span data-stu-id="06229-142">To provide a default value for the Web Service Parameter, click the icon to the right of the name, select "Provide default value", enter a value (for example, "container1/output1.csv"), and press the **Enter** key.</span></span>
   
   ![Web Service-Parameter][parameter]
6. <span data-ttu-id="06229-144">Klicka på **Run** (Kör).</span><span class="sxs-lookup"><span data-stu-id="06229-144">Click **Run**.</span></span> 
7. <span data-ttu-id="06229-145">Klicka på **distribuera webbtjänsten** och välj **distribuera webbtjänsten [klassisk]** eller **distribuera webbtjänsten [ny]** att distribuera webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-145">Click **Deploy Web Service** and select **Deploy Web Service [Classic]** or **Deploy Web Service [New]** to deploy the web service.</span></span>

> [!NOTE] 
> <span data-ttu-id="06229-146">Om du vill distribuera en ny webbtjänst måste du ha tillräckliga behörigheter i prenumerationen som du distribuerar webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-146">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="06229-147">Mer information finns i [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="06229-147">For more information see, [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="06229-148">Användaren av webbtjänsten kan nu ange ett nytt mål för den [exportera Data] [ writer] modulen vid åtkomst till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="06229-148">The user of the web service can now specify a new destination for the [Export Data][writer] module when accessing the web service.</span></span>

## <a name="more-information"></a><span data-ttu-id="06229-149">Mer information</span><span class="sxs-lookup"><span data-stu-id="06229-149">More information</span></span>
<span data-ttu-id="06229-150">En mer detaljerad exempel finns i [Webbtjänstparametrar](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) post i den [Machine Learning blogg](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span><span class="sxs-lookup"><span data-stu-id="06229-150">For a more detailed example, see the [Web Service Parameters](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) entry in the [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).</span></span>

<span data-ttu-id="06229-151">Mer information om hur du använder en Machine Learning-webbtjänst finns [använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="06229-151">For more information on accessing a Machine Learning web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

