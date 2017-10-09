---
title: "aaaDeploying en ny webbtjänst i Azure Machine Learning | Microsoft Docs"
description: "hello Arbetsflödesbaserade för att distribuera en ARM webbtjänst"
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: a358b04f-0d08-4d50-820e-eeac971854cf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: v-donglo
ROBOTS: NOINDEX
redirect_url: machine-learning-publish-a-machine-learning-web-service
redirect_document_id: True
ms.openlocfilehash: 2cbfda44b97a6b992fbdfdfb0c761e6c9e169035
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-new-web-service"></a><span data-ttu-id="3abb4-103">Distribuera en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="3abb4-103">Deploy a new web service</span></span>
<span data-ttu-id="3abb4-104">Microsoft Azure Machine learning nu innehåller webbtjänster som baseras på [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tillåter nya fakturering alternativ och distribuera din web service toomultiple regioner.</span><span class="sxs-lookup"><span data-stu-id="3abb4-104">Microsoft Azure Machine learning now provides web services that are based on [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) allowing for new billing plan options and deploying your web service toomultiple regions.</span></span>

<span data-ttu-id="3abb4-105">hello Allmänt arbetsflöde toodeploy en webbtjänst med hjälp av Microsoft Azure Machine Learning-webbtjänster är:</span><span class="sxs-lookup"><span data-stu-id="3abb4-105">hello general workflow toodeploy a web service using Microsoft Azure Machine Learning Web Services is:</span></span>

* <span data-ttu-id="3abb4-106">Skapa en prediktiv experiment</span><span class="sxs-lookup"><span data-stu-id="3abb4-106">Create a predictive experiment</span></span>
* <span data-ttu-id="3abb4-107">distribuera den</span><span class="sxs-lookup"><span data-stu-id="3abb4-107">deploy it</span></span>
* <span data-ttu-id="3abb4-108">konfigurera dess namn</span><span class="sxs-lookup"><span data-stu-id="3abb4-108">configure its name</span></span>
* <span data-ttu-id="3abb4-109">faktureringsavtal</span><span class="sxs-lookup"><span data-stu-id="3abb4-109">billing plan</span></span>
* <span data-ttu-id="3abb4-110">testa den</span><span class="sxs-lookup"><span data-stu-id="3abb4-110">test it</span></span>
* <span data-ttu-id="3abb4-111">använda den.</span><span class="sxs-lookup"><span data-stu-id="3abb4-111">consume it.</span></span>

<span data-ttu-id="3abb4-112">hello följande bild illustrerar arbetsflödet för hello.</span><span class="sxs-lookup"><span data-stu-id="3abb4-112">hello following graphic illustrates hello workflow.</span></span>

![Arbetsflöde för distribution av Web service][1]

## <a name="deploy-web-service-from-studio"></a><span data-ttu-id="3abb4-114">Distribuera webbtjänsten från Studio</span><span class="sxs-lookup"><span data-stu-id="3abb4-114">Deploy web service from Studio</span></span>
<span data-ttu-id="3abb4-115">toodeploy ett experiment som en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3abb4-115">toodeploy an experiment as a new web service.</span></span> <span data-ttu-id="3abb4-116">Logga in på hello Machine Learning Studio och skapa en ny förutsägande webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3abb4-116">Sign into hello Machine Learning Studio and create a new predictive web service.</span></span> 

<span data-ttu-id="3abb4-117">**Obs**: Om du redan har distribuerat ett experiment som en klassiska webbtjänst kan du distribuera den som en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3abb4-117">**Note**: If you have already deployed an experiment as a classic web service you cannot deploy it as a new web service.</span></span>

<span data-ttu-id="3abb4-118">Klicka på **kör** längst hello hello experimentera arbetsytan och klicka sedan på **distribuera webbtjänsten** och **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-118">Click **Run** at hello bottom of hello experiment canvas and then click **Deploy Web Service** and **Deploy Web Service [New]**.</span></span> <span data-ttu-id="3abb4-119">hello öppnas distribution av hello Machine Learning-webbtjänst manager.</span><span class="sxs-lookup"><span data-stu-id="3abb4-119">hello deployment page of hello Machine Learning Web Service manager will open.</span></span>

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a><span data-ttu-id="3abb4-120">Machine Learning Web Service Manager distribuera Experiment sida</span><span class="sxs-lookup"><span data-stu-id="3abb4-120">Machine Learning Web Service Manager Deploy Experiment Page</span></span>
<span data-ttu-id="3abb4-121">Ange ett namn för webbtjänsten hello hello distribuera Experiment på sidan.</span><span class="sxs-lookup"><span data-stu-id="3abb4-121">On hello Deploy Experiment page, enter a name for hello web service.</span></span>
<span data-ttu-id="3abb4-122">Välj en prisnivå plan.</span><span class="sxs-lookup"><span data-stu-id="3abb4-122">Select a pricing plan.</span></span> <span data-ttu-id="3abb4-123">Om du har en befintlig prisavtal kan du välja det annars skapa du en ny prisplan för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3abb4-123">If you have an existing pricing plan you can select it, otherwise you must create a new price plan for hello service.</span></span> 

1. <span data-ttu-id="3abb4-124">I hello **prisplan** listrutan väljer du ett befintligt schema eller hello **Välj ny plan** alternativet.</span><span class="sxs-lookup"><span data-stu-id="3abb4-124">In hello **Price Plan** drop down, select an existing plan or select hello **Select new plan** option.</span></span>
2. <span data-ttu-id="3abb4-125">I **schemanamn**, Skriv ett namn som identifierar hello plan på fakturan.</span><span class="sxs-lookup"><span data-stu-id="3abb4-125">In **Plan Name**, type a name that will identify hello plan on your bill.</span></span>
3. <span data-ttu-id="3abb4-126">Välj en av hello **månatliga planera nivåer**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-126">Select one of hello **Monthly Plan Tiers**.</span></span> <span data-ttu-id="3abb4-127">Observera att hello plan nivåerna standard toohello planer för standardregion och webbtjänsten är distribuerade toothat region.</span><span class="sxs-lookup"><span data-stu-id="3abb4-127">Note that hello plan tiers default toohello plans for your default region and your web service is deployed toothat region.</span></span>

<span data-ttu-id="3abb4-128">Klicka på **distribuera** och hello Snabbstart för webbtjänsten öppnas.</span><span class="sxs-lookup"><span data-stu-id="3abb4-128">Click **Deploy** and hello Quickstart page for your web service opens.</span></span>

## <a name="quickstart-page"></a><span data-ttu-id="3abb4-129">Sidan Snabbstart</span><span class="sxs-lookup"><span data-stu-id="3abb4-129">Quickstart page</span></span>
<span data-ttu-id="3abb4-130">hello-webbtjänstsida Quickstart ger dig åtkomst och vägledning på hello vanligaste uppgifter som du utför när du har skapat en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="3abb4-130">hello web service Quickstart page gives you access and guidance on hello most common tasks you will perform after creating a new web service.</span></span> <span data-ttu-id="3abb4-131">Härifrån kan du enkelt komma åt båda hello **Test** sidan och **förbruka** sidan.</span><span class="sxs-lookup"><span data-stu-id="3abb4-131">From here you can easily access both hello **Test** page and **Consume** page.</span></span>

## <a name="testing-your-web-service"></a><span data-ttu-id="3abb4-132">Testa din webbtjänst</span><span class="sxs-lookup"><span data-stu-id="3abb4-132">Testing your web service</span></span>
<span data-ttu-id="3abb4-133">Klicka på Testa webbtjänsten under vanliga uppgifter från hello Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="3abb4-133">From hello Quickstart page, click Test web service under common tasks.</span></span>   

<span data-ttu-id="3abb4-134">tootest hello webbtjänsten som ett svar på begäranden tjänsten (RR):</span><span class="sxs-lookup"><span data-stu-id="3abb4-134">tootest hello web service as a Request-Response Service (RRS):</span></span>

* <span data-ttu-id="3abb4-135">Klicka på **Test** hello menyraden.</span><span class="sxs-lookup"><span data-stu-id="3abb4-135">Click **Test** on hello menu bar.</span></span>
* <span data-ttu-id="3abb4-136">Klicka på **begäran och svar**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-136">Click **Request-Response**.</span></span>
* <span data-ttu-id="3abb4-137">Ange lämpliga värden för hello indatakolumnerna av experimentet.</span><span class="sxs-lookup"><span data-stu-id="3abb4-137">Enter appropriate values for hello input columns of your experiment.</span></span>
* <span data-ttu-id="3abb4-138">Klicka på Testa **begäran och svar**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-138">Click Test **Request-Response**.</span></span>

<span data-ttu-id="3abb4-139">Du resultat visas hello höger på sidan hello.</span><span class="sxs-lookup"><span data-stu-id="3abb4-139">You results will display on hello right hand side of hello page.</span></span>

<span data-ttu-id="3abb4-140">tootest en Batch Execution Service BES-webbtjänsten, ska du använda en CSV-fil:</span><span class="sxs-lookup"><span data-stu-id="3abb4-140">tootest a Batch Execution Service (BES) web service, you will use a CSV file:</span></span>

* <span data-ttu-id="3abb4-141">Klicka på **Test** hello menyraden.</span><span class="sxs-lookup"><span data-stu-id="3abb4-141">Click **Test** on hello menu bar.</span></span>
* <span data-ttu-id="3abb4-142">Klicka på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-142">Click **Batch**.</span></span>
* <span data-ttu-id="3abb4-143">Klicka på Bläddra och navigera tooyour exempeldatafil under dina indata.</span><span class="sxs-lookup"><span data-stu-id="3abb4-143">Under your input, click Browse and navigate tooyour sample data file.</span></span>
* <span data-ttu-id="3abb4-144">Klicka på **Test**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-144">Click **Test**.</span></span>

<span data-ttu-id="3abb4-145">hello status för testet visas under **testa batchjobb**.</span><span class="sxs-lookup"><span data-stu-id="3abb4-145">hello status of your test is displayed under **Test Batch Jobs**.</span></span>

## <a name="consuming-your-web-service"></a><span data-ttu-id="3abb4-146">Använda webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="3abb4-146">Consuming your Web Service</span></span>
<span data-ttu-id="3abb4-147">När de distribueras som en webbtjänst ger Azure Machine Learning-experiment en REST-API som kan användas av en mängd olika plattformar och enheter.</span><span class="sxs-lookup"><span data-stu-id="3abb4-147">When deployed as a web service, Azure Machine Learning experiments provide a REST API that can be consumed by a wide range of devices and platforms.</span></span> <span data-ttu-id="3abb4-148">Det beror på att hello enkla REST-API accepterar och svarar med JSON-formaterade meddelanden.</span><span class="sxs-lookup"><span data-stu-id="3abb4-148">This is because hello simple REST API accepts and responds with JSON formatted messages.</span></span> <span data-ttu-id="3abb4-149">hello Azure Machine Learning-portalen innehåller koden som du kan använda toocall hello-webbtjänsten i R, C# och Python.</span><span class="sxs-lookup"><span data-stu-id="3abb4-149">hello Azure Machine Learning portal provides code that can be used toocall hello web service in R, C#, and Python.</span></span>

<span data-ttu-id="3abb4-150">På sidan för användning av hello du:</span><span class="sxs-lookup"><span data-stu-id="3abb4-150">On hello Consuming page you can find:</span></span>

* <span data-ttu-id="3abb4-151">hello API-nyckeln och URI: er för förbrukning av webbtjänsten i appar.</span><span class="sxs-lookup"><span data-stu-id="3abb4-151">hello API key and URI's for consuming web service in apps.</span></span>
* <span data-ttu-id="3abb4-152">Excel och web app mallar tookick starta förbrukning-processen.</span><span class="sxs-lookup"><span data-stu-id="3abb4-152">Excel and web app templates tookick start your consumption process.</span></span>
* <span data-ttu-id="3abb4-153">Exempelkoden i C#, python och R tooget du startade.</span><span class="sxs-lookup"><span data-stu-id="3abb4-153">Sample code in C#, python, and R tooget you started.</span></span>

<span data-ttu-id="3abb4-154">Mer information om att konsumera webbtjänster finns [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="3abb4-154">For more information on consuming web services, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3abb4-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3abb4-155">Next Steps</span></span>
<span data-ttu-id="3abb4-156">Mer information om att konsumera webbtjänster finns:</span><span class="sxs-lookup"><span data-stu-id="3abb4-156">For more information on consuming web services, see:</span></span>

[<span data-ttu-id="3abb4-157">Hur tooconsume en Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="3abb4-157">How tooconsume an Azure Machine Learning Web service</span></span>](machine-learning-consume-web-services.md)

[<span data-ttu-id="3abb4-158">Azure Machine Learning-webbtjänster: Distribution och användning</span><span class="sxs-lookup"><span data-stu-id="3abb4-158">Azure Machine Learning Web Services: Deployment and consumption</span></span>](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
