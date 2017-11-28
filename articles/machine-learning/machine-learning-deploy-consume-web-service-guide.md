---
title: "Azure Machine Learning-webbtjänster: Distribution och användning | Microsoft Docs"
description: "Resurser för att distribuera och använda webbtjänster."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 539c2abb053a0f981be0374defe45cf4d96b740b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a><span data-ttu-id="e3216-103">Azure Machine Learning-webbtjänster: Distribution och användning</span><span class="sxs-lookup"><span data-stu-id="e3216-103">Azure Machine Learning Web Services: Deployment and consumption</span></span>
<span data-ttu-id="e3216-104">Du kan använda Azure Machine Learning toodeploy maskininlärning arbetsflöden och modeller som webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="e3216-104">You can use Azure Machine Learning toodeploy machine-learning workflows and models as web services.</span></span> <span data-ttu-id="e3216-105">Dessa webbtjänster kan sedan använda toocall hello machine learning-modeller från program över hello Internet toodo förutsägelser i realtid eller i batchläge.</span><span class="sxs-lookup"><span data-stu-id="e3216-105">These web services can then be used toocall hello machine-learning models from applications over hello Internet toodo predictions in real time or in batch mode.</span></span> <span data-ttu-id="e3216-106">Eftersom hello webbtjänster RESTful kan anropa du dem från olika programmeringsspråk språk och plattformar, till exempel .NET och Java, och program som Excel.</span><span class="sxs-lookup"><span data-stu-id="e3216-106">Because hello web services are RESTful, you can call them from various programming languages and platforms, such as .NET and Java, and from applications, such as Excel.</span></span>

<span data-ttu-id="e3216-107">hello nästa avsnitt innehåller länkar toowalkthroughs, kod och dokumentation toohelp komma igång.</span><span class="sxs-lookup"><span data-stu-id="e3216-107">hello next sections provide links toowalkthroughs, code, and documentation toohelp get you started.</span></span>

## <a name="deploy-a-web-service"></a><span data-ttu-id="e3216-108">Distribuera en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e3216-108">Deploy a web service</span></span>
### <a name="with-azure-machine-learning-studio"></a><span data-ttu-id="e3216-109">Med Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="e3216-109">With Azure Machine Learning Studio</span></span>
<span data-ttu-id="e3216-110">Machine Learning Studio och hello Microsoft Azure Machine Learning-webbtjänster portalen kan du distribuera och hantera en webbtjänst utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="e3216-110">Machine Learning Studio and hello Microsoft Azure Machine Learning Web Services portal help you deploy and manage a web service without writing code.</span></span>

<span data-ttu-id="e3216-111">hello följande länkar ger allmän Information om hur toodeploy en ny webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="e3216-111">hello following links provide general Information about how toodeploy a new web service:</span></span>

* <span data-ttu-id="e3216-112">För en översikt över hur toodeploy en ny webbtjänst som baseras på Azure Resource Manager, se [distribuera en ny webbtjänst](machine-learning-webservice-deploy-a-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="e3216-112">For an overview about how toodeploy a new web service that's based on Azure Resource Manager, see [Deploy a new web service](machine-learning-webservice-deploy-a-web-service.md).</span></span>
* <span data-ttu-id="e3216-113">En genomgång om hur toodeploy en webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="e3216-113">For a walkthrough about how toodeploy a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="e3216-114">En fullständig genomgång om hur toocreate och distribuera en webbtjänst kan se [genomgången steg 1: skapa en arbetsyta i Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="e3216-114">For a full walkthrough about how toocreate and deploy a web service, see [Walkthrough Step 1: Create a Machine Learning workspace](machine-learning-walkthrough-1-create-ml-workspace.md).</span></span>
* <span data-ttu-id="e3216-115">Vissa exempel som distribuerar en webbtjänst finns:</span><span class="sxs-lookup"><span data-stu-id="e3216-115">For specific examples that deploy a web service, see:</span></span>

  * [<span data-ttu-id="e3216-116">Genomgång steg 5: Distribuera hello Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e3216-116">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
  * [<span data-ttu-id="e3216-117">Hur toodeploy en web service toomultiple regioner</span><span class="sxs-lookup"><span data-stu-id="e3216-117">How toodeploy a web service toomultiple regions</span></span>](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a><span data-ttu-id="e3216-118">Med resursprovidern för web services API: er (Azure Resource Manager API: er)</span><span class="sxs-lookup"><span data-stu-id="e3216-118">With web services resource provider APIs (Azure Resource Manager APIs)</span></span>
<span data-ttu-id="e3216-119">hello Azure Machine Learning resource provider för webbtjänster kan distribution och hantering av webbtjänster med hjälp av REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="e3216-119">hello Azure Machine Learning resource provider for web services enables deployment and management of web services by using REST API calls.</span></span> <span data-ttu-id="e3216-120">Mer information finns i [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) referens.</span><span class="sxs-lookup"><span data-stu-id="e3216-120">For additional details, see the [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) reference.</span></span>

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a><span data-ttu-id="e3216-121">Med PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="e3216-121">With PowerShell cmdlets</span></span>
<span data-ttu-id="e3216-122">Azure Machine Learning resource provider för webbtjänster kan distribution och hantering av webbtjänster med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="e3216-122">Azure Machine Learning resource provider for web services enables deployment and management of web services by using PowerShell cmdlets.</span></span>

<span data-ttu-id="e3216-123">toouse hello cmdlets måste du först logga in tooyour Azure-konto från inom hello PowerShell-miljö med hjälp av hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e3216-123">toouse hello cmdlets, you must first sign in tooyour Azure account from within hello PowerShell environment by using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="e3216-124">Om du inte känner till hur toocall PowerShell-kommandon som baseras på Resource Manager, se [med hjälp av Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span><span class="sxs-lookup"><span data-stu-id="e3216-124">If you are unfamiliar with how toocall PowerShell commands that are based on Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span></span>

<span data-ttu-id="e3216-125">tooexport din förutsägande experimentera använder [den här exempelkoden](https://github.com/ritwik20/AzureML-WebServices).</span><span class="sxs-lookup"><span data-stu-id="e3216-125">tooexport your predictive experiment, use [this sample code](https://github.com/ritwik20/AzureML-WebServices).</span></span> <span data-ttu-id="e3216-126">När du har skapat hello .exe-fil från hello kod skriver du:</span><span class="sxs-lookup"><span data-stu-id="e3216-126">After you create hello .exe file from hello code, you can type:</span></span>

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

<span data-ttu-id="e3216-127">Hello-program körs skapas en web service JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="e3216-127">Running hello application creates a web service JSON template.</span></span> <span data-ttu-id="e3216-128">toouse hello mallen toodeploy en webbtjänst, måste du lägga till hello följande information:</span><span class="sxs-lookup"><span data-stu-id="e3216-128">toouse hello template toodeploy a web service, you must add hello following information:</span></span>

* <span data-ttu-id="e3216-129">Lagringskontonamn och nyckel</span><span class="sxs-lookup"><span data-stu-id="e3216-129">Storage account name and key</span></span>

    <span data-ttu-id="e3216-130">Du kan hämta hello lagringskontonamn och nyckel från antingen hello [Azure-portalen](https://portal.azure.com/) eller hello [klassiska Azure-portalen](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="e3216-130">You can get hello storage account name and key from either hello [Azure portal](https://portal.azure.com/) or hello [Azure classic portal](http://manage.windowsazure.com/).</span></span>
* <span data-ttu-id="e3216-131">Åtagande plan-ID</span><span class="sxs-lookup"><span data-stu-id="e3216-131">Commitment plan ID</span></span>

    <span data-ttu-id="e3216-132">Du kan hämta hello plan ID från hello [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal genom att logga in och klicka på ett namn för energischema.</span><span class="sxs-lookup"><span data-stu-id="e3216-132">You can get hello plan ID from hello [Azure Machine Learning Web Services](https://services.azureml.net) portal by signing in and clicking a plan name.</span></span>

<span data-ttu-id="e3216-133">Lägg till dem toohello JSON-mall som underordnade till hello *egenskaper* nod på hello samma nivå som hello *MachineLearningWorkspace* nod.</span><span class="sxs-lookup"><span data-stu-id="e3216-133">Add them toohello JSON template as children of hello *Properties* node at hello same level as hello *MachineLearningWorkspace* node.</span></span>

<span data-ttu-id="e3216-134">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="e3216-134">Here's an example:</span></span>

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

<span data-ttu-id="e3216-135">Se följande artiklar hello och exempelkod för ytterligare information:</span><span class="sxs-lookup"><span data-stu-id="e3216-135">See hello following articles and sample code for additional details:</span></span>

* <span data-ttu-id="e3216-136">[Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference på MSDN</span><span class="sxs-lookup"><span data-stu-id="e3216-136">[Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN</span></span>
* <span data-ttu-id="e3216-137">Exempel [genomgången](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) på GitHub</span><span class="sxs-lookup"><span data-stu-id="e3216-137">Sample [walkthrough](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) on GitHub</span></span>

## <a name="consume-hello-web-services"></a><span data-ttu-id="e3216-138">Använda hello-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="e3216-138">Consume hello web services</span></span>
### <a name="from-hello-azure-machine-learning-web-services-ui-testing"></a><span data-ttu-id="e3216-139">Från hello Azure Machine Learning Web Services-Användargränssnittet (testar)</span><span class="sxs-lookup"><span data-stu-id="e3216-139">From hello Azure Machine Learning Web Services UI (Testing)</span></span>
<span data-ttu-id="e3216-140">Du kan testa webbtjänsten från hello Azure Machine Learning-webbtjänster portalen.</span><span class="sxs-lookup"><span data-stu-id="e3216-140">You can test your web service from hello Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="e3216-141">Detta inkluderar testning hello svar på begäranden tjänsten (RR) och Batch Execution service (BES)-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="e3216-141">This includes testing hello Request-Response service (RRS) and Batch Execution service (BES) interfaces.</span></span>

* [<span data-ttu-id="e3216-142">Distribuera en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e3216-142">Deploy a new web service</span></span>](machine-learning-webservice-deploy-a-web-service.md)
* [<span data-ttu-id="e3216-143">Distribuera en Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e3216-143">Deploy an Azure Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
* [<span data-ttu-id="e3216-144">Genomgång steg 5: Distribuera hello Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="e3216-144">Walkthrough Step 5: Deploy hello Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a><span data-ttu-id="e3216-145">Från Excel</span><span class="sxs-lookup"><span data-stu-id="e3216-145">From Excel</span></span>
<span data-ttu-id="e3216-146">Du kan hämta en Excel-mall som förbrukar hello-webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="e3216-146">You can download an Excel template that consumes hello web service:</span></span>

* [<span data-ttu-id="e3216-147">Använda en Azure Machine Learning-webbtjänst från Excel</span><span class="sxs-lookup"><span data-stu-id="e3216-147">Consuming an Azure Machine Learning web service from Excel</span></span>](machine-learning-consuming-from-excel.md)
* [<span data-ttu-id="e3216-148">Excel-tillägg för Azure Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="e3216-148">Excel add-in for Azure Machine Learning Web Services</span></span>](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a><span data-ttu-id="e3216-149">En REST-baserad klient</span><span class="sxs-lookup"><span data-stu-id="e3216-149">From a REST-based client</span></span>
<span data-ttu-id="e3216-150">Azure Machine Learning-webbtjänster är RESTful-API: er.</span><span class="sxs-lookup"><span data-stu-id="e3216-150">Azure Machine Learning Web Services are RESTful APIs.</span></span> <span data-ttu-id="e3216-151">Du kan använda dessa API: er från olika plattformar, till exempel .NET, Python, R, Java, etc. hello **förbruka** för webbtjänsten på hello [Microsoft Azure Machine Learning-webbtjänster portal](https://services.azureml.net) har exempel kod som kan hjälpa dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="e3216-151">You can consume these APIs from various platforms, such as .NET, Python, R, Java, etc. hello **Consume** page for your web service on hello [Microsoft Azure Machine Learning Web Services portal](https://services.azureml.net) has sample code that can help you get started.</span></span> <span data-ttu-id="e3216-152">Mer information finns i [hur tooconsume en Azure Machine Learning-webbtjänst](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="e3216-152">For more information, see [How tooconsume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>
