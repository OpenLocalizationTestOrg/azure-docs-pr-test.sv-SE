---
title: "Distribuera en ny webbtjänst i Azure Machine Learning | Microsoft Docs"
description: "Arbetsflöde för att distribuera en ARM-baserad webbtjänst"
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
redirect_document_id: TRUE
ms.openlocfilehash: 1415709f9da2bb2cce859af9feb0ec15c1fa5801
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-new-web-service"></a><span data-ttu-id="12e69-103">Distribuera en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="12e69-103">Deploy a new web service</span></span>
<span data-ttu-id="12e69-104">Microsoft Azure Machine learning nu innehåller webbtjänster som baseras på [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) tillåter nya fakturering alternativ och distribuera webbtjänsten till flera regioner.</span><span class="sxs-lookup"><span data-stu-id="12e69-104">Microsoft Azure Machine learning now provides web services that are based on [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) allowing for new billing plan options and deploying your web service to multiple regions.</span></span>

<span data-ttu-id="12e69-105">Det allmänna arbetsflödet för att distribuera en webbtjänst med hjälp av Microsoft Azure Machine Learning-webbtjänster är:</span><span class="sxs-lookup"><span data-stu-id="12e69-105">The general workflow to deploy a web service using Microsoft Azure Machine Learning Web Services is:</span></span>

* <span data-ttu-id="12e69-106">Skapa en prediktiv experiment</span><span class="sxs-lookup"><span data-stu-id="12e69-106">Create a predictive experiment</span></span>
* <span data-ttu-id="12e69-107">distribuera den</span><span class="sxs-lookup"><span data-stu-id="12e69-107">deploy it</span></span>
* <span data-ttu-id="12e69-108">konfigurera dess namn</span><span class="sxs-lookup"><span data-stu-id="12e69-108">configure its name</span></span>
* <span data-ttu-id="12e69-109">faktureringsavtal</span><span class="sxs-lookup"><span data-stu-id="12e69-109">billing plan</span></span>
* <span data-ttu-id="12e69-110">testa den</span><span class="sxs-lookup"><span data-stu-id="12e69-110">test it</span></span>
* <span data-ttu-id="12e69-111">använda den.</span><span class="sxs-lookup"><span data-stu-id="12e69-111">consume it.</span></span>

<span data-ttu-id="12e69-112">Följande bild illustrerar arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="12e69-112">The following graphic illustrates the workflow.</span></span>

![Arbetsflöde för distribution av Web service][1]

## <a name="deploy-web-service-from-studio"></a><span data-ttu-id="12e69-114">Distribuera webbtjänsten från Studio</span><span class="sxs-lookup"><span data-stu-id="12e69-114">Deploy web service from Studio</span></span>
<span data-ttu-id="12e69-115">Att distribuera ett experiment som en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="12e69-115">To deploy an experiment as a new web service.</span></span> <span data-ttu-id="12e69-116">Logga in på Machine Learning Studio och skapa en ny förutsägande webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="12e69-116">Sign into the Machine Learning Studio and create a new predictive web service.</span></span> 

<span data-ttu-id="12e69-117">**Obs**: Om du redan har distribuerat ett experiment som en klassiska webbtjänst kan du distribuera den som en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="12e69-117">**Note**: If you have already deployed an experiment as a classic web service you cannot deploy it as a new web service.</span></span>

<span data-ttu-id="12e69-118">Klicka på **kör** längst ned i experimentet arbetsytan och klicka sedan på **distribuera webbtjänsten** och **distribuera webbtjänsten [ny]**.</span><span class="sxs-lookup"><span data-stu-id="12e69-118">Click **Run** at the bottom of the experiment canvas and then click **Deploy Web Service** and **Deploy Web Service [New]**.</span></span> <span data-ttu-id="12e69-119">Sidan distribution av Machine Learning-webbtjänst manager öppnas.</span><span class="sxs-lookup"><span data-stu-id="12e69-119">The deployment page of the Machine Learning Web Service manager will open.</span></span>

## <a name="machine-learning-web-service-manager-deploy-experiment-page"></a><span data-ttu-id="12e69-120">Machine Learning Web Service Manager distribuera Experiment sida</span><span class="sxs-lookup"><span data-stu-id="12e69-120">Machine Learning Web Service Manager Deploy Experiment Page</span></span>
<span data-ttu-id="12e69-121">Ange ett namn för webbtjänsten på sidan distribuera Experiment.</span><span class="sxs-lookup"><span data-stu-id="12e69-121">On the Deploy Experiment page, enter a name for the web service.</span></span>
<span data-ttu-id="12e69-122">Välj en prisnivå plan.</span><span class="sxs-lookup"><span data-stu-id="12e69-122">Select a pricing plan.</span></span> <span data-ttu-id="12e69-123">Om du har en befintlig prisavtal kan du välja det måste annars du skapa en ny prisplan för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="12e69-123">If you have an existing pricing plan you can select it, otherwise you must create a new price plan for the service.</span></span> 

1. <span data-ttu-id="12e69-124">I den **prisplan** listrutan väljer du ett befintligt schema eller den **Välj ny plan** alternativet.</span><span class="sxs-lookup"><span data-stu-id="12e69-124">In the **Price Plan** drop down, select an existing plan or select the **Select new plan** option.</span></span>
2. <span data-ttu-id="12e69-125">I **schemanamn**, Skriv ett namn som identifierar planen på fakturan.</span><span class="sxs-lookup"><span data-stu-id="12e69-125">In **Plan Name**, type a name that will identify the plan on your bill.</span></span>
3. <span data-ttu-id="12e69-126">Välj en av de **månatliga planera nivåer**.</span><span class="sxs-lookup"><span data-stu-id="12e69-126">Select one of the **Monthly Plan Tiers**.</span></span> <span data-ttu-id="12e69-127">Observera att planen nivåerna standard att planer för standardregion och webbtjänsten har distribuerats till den regionen.</span><span class="sxs-lookup"><span data-stu-id="12e69-127">Note that the plan tiers default to the plans for your default region and your web service is deployed to that region.</span></span>

<span data-ttu-id="12e69-128">Klicka på **distribuera** och öppnas sidan Snabbstart för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="12e69-128">Click **Deploy** and the Quickstart page for your web service opens.</span></span>

## <a name="quickstart-page"></a><span data-ttu-id="12e69-129">Sidan Snabbstart</span><span class="sxs-lookup"><span data-stu-id="12e69-129">Quickstart page</span></span>
<span data-ttu-id="12e69-130">Sidan Snabbstart service ger dig åtkomst och riktlinjer för de vanligaste uppgifterna som du utför när du har skapat en ny webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="12e69-130">The web service Quickstart page gives you access and guidance on the most common tasks you will perform after creating a new web service.</span></span> <span data-ttu-id="12e69-131">Härifrån du enkelt kan komma åt både den **Test** sidan och **förbruka** sidan.</span><span class="sxs-lookup"><span data-stu-id="12e69-131">From here you can easily access both the **Test** page and **Consume** page.</span></span>

## <a name="testing-your-web-service"></a><span data-ttu-id="12e69-132">Testa din webbtjänst</span><span class="sxs-lookup"><span data-stu-id="12e69-132">Testing your web service</span></span>
<span data-ttu-id="12e69-133">Sidan Snabbstart klickar du på Test webbtjänsten under vanliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="12e69-133">From the Quickstart page, click Test web service under common tasks.</span></span>   

<span data-ttu-id="12e69-134">Så här testar webbtjänsten som ett svar på begäranden tjänsten (RR):</span><span class="sxs-lookup"><span data-stu-id="12e69-134">To test the web service as a Request-Response Service (RRS):</span></span>

* <span data-ttu-id="12e69-135">Klicka på **Test** på menyraden.</span><span class="sxs-lookup"><span data-stu-id="12e69-135">Click **Test** on the menu bar.</span></span>
* <span data-ttu-id="12e69-136">Klicka på **begäran och svar**.</span><span class="sxs-lookup"><span data-stu-id="12e69-136">Click **Request-Response**.</span></span>
* <span data-ttu-id="12e69-137">Ange lämpliga värden för indatakolumnerna av experimentet.</span><span class="sxs-lookup"><span data-stu-id="12e69-137">Enter appropriate values for the input columns of your experiment.</span></span>
* <span data-ttu-id="12e69-138">Klicka på Testa **begäran och svar**.</span><span class="sxs-lookup"><span data-stu-id="12e69-138">Click Test **Request-Response**.</span></span>

<span data-ttu-id="12e69-139">Du resultat visas till höger på sidan.</span><span class="sxs-lookup"><span data-stu-id="12e69-139">You results will display on the right hand side of the page.</span></span>

<span data-ttu-id="12e69-140">Om du vill testa en Batch Execution Service BES-webbtjänsten använder du en CSV-fil:</span><span class="sxs-lookup"><span data-stu-id="12e69-140">To test a Batch Execution Service (BES) web service, you will use a CSV file:</span></span>

* <span data-ttu-id="12e69-141">Klicka på **Test** på menyraden.</span><span class="sxs-lookup"><span data-stu-id="12e69-141">Click **Test** on the menu bar.</span></span>
* <span data-ttu-id="12e69-142">Klicka på **Batch**.</span><span class="sxs-lookup"><span data-stu-id="12e69-142">Click **Batch**.</span></span>
* <span data-ttu-id="12e69-143">Klicka på Bläddra och navigera till din exempeldatafil under dina indata.</span><span class="sxs-lookup"><span data-stu-id="12e69-143">Under your input, click Browse and navigate to your sample data file.</span></span>
* <span data-ttu-id="12e69-144">Klicka på **Test**.</span><span class="sxs-lookup"><span data-stu-id="12e69-144">Click **Test**.</span></span>

<span data-ttu-id="12e69-145">Status för testet visas under **testa batchjobb**.</span><span class="sxs-lookup"><span data-stu-id="12e69-145">The status of your test is displayed under **Test Batch Jobs**.</span></span>

## <a name="consuming-your-web-service"></a><span data-ttu-id="12e69-146">Använda webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="12e69-146">Consuming your Web Service</span></span>
<span data-ttu-id="12e69-147">När de distribueras som en webbtjänst ger Azure Machine Learning-experiment en REST-API som kan användas av en mängd olika plattformar och enheter.</span><span class="sxs-lookup"><span data-stu-id="12e69-147">When deployed as a web service, Azure Machine Learning experiments provide a REST API that can be consumed by a wide range of devices and platforms.</span></span> <span data-ttu-id="12e69-148">Detta beror på att enkelt REST API accepterar och svarar med JSON-formaterade meddelanden.</span><span class="sxs-lookup"><span data-stu-id="12e69-148">This is because the simple REST API accepts and responds with JSON formatted messages.</span></span> <span data-ttu-id="12e69-149">Azure Machine Learning-portalen innehåller kod som kan användas för att anropa webbtjänsten i R, C# och Python.</span><span class="sxs-lookup"><span data-stu-id="12e69-149">The Azure Machine Learning portal provides code that can be used to call the web service in R, C#, and Python.</span></span>

<span data-ttu-id="12e69-150">På sidan förbrukning hittar du:</span><span class="sxs-lookup"><span data-stu-id="12e69-150">On the Consuming page you can find:</span></span>

* <span data-ttu-id="12e69-151">API-nyckel och URI: er för förbrukning av webbtjänsten i appar.</span><span class="sxs-lookup"><span data-stu-id="12e69-151">The API key and URI's for consuming web service in apps.</span></span>
* <span data-ttu-id="12e69-152">Mallar för Excel och web app kan förbättra starta förbrukning-processen.</span><span class="sxs-lookup"><span data-stu-id="12e69-152">Excel and web app templates to kick start your consumption process.</span></span>
* <span data-ttu-id="12e69-153">Exempelkoden i C#, python och R att komma igång.</span><span class="sxs-lookup"><span data-stu-id="12e69-153">Sample code in C#, python, and R to get you started.</span></span>

<span data-ttu-id="12e69-154">Mer information om att konsumera webbtjänster finns [använda en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="12e69-154">For more information on consuming web services, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="12e69-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12e69-155">Next Steps</span></span>
<span data-ttu-id="12e69-156">Mer information om att konsumera webbtjänster finns:</span><span class="sxs-lookup"><span data-stu-id="12e69-156">For more information on consuming web services, see:</span></span>

[<span data-ttu-id="12e69-157">Använda en Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="12e69-157">How to consume an Azure Machine Learning Web service</span></span>](machine-learning-consume-web-services.md)

[<span data-ttu-id="12e69-158">Azure Machine Learning-webbtjänster: Distribution och användning</span><span class="sxs-lookup"><span data-stu-id="12e69-158">Azure Machine Learning Web Services: Deployment and consumption</span></span>](machine-learning-deploy-consume-web-service-guide.md)

<!--Image references-->
[1]: ./media/machine-learning-webservice-deploy-a-web-service/armdeploymentworkflow.png


<!--links-->
