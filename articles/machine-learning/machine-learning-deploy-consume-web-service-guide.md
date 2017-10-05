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
ms.openlocfilehash: 18edabe267ec06c08074d7a7a6d71435cedc8489
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a><span data-ttu-id="6ca72-103">Azure Machine Learning-webbtjänster: Distribution och användning</span><span class="sxs-lookup"><span data-stu-id="6ca72-103">Azure Machine Learning Web Services: Deployment and consumption</span></span>
<span data-ttu-id="6ca72-104">Du kan använda Azure Machine Learning för att distribuera machine learning arbetsflöden och modeller som webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="6ca72-104">You can use Azure Machine Learning to deploy machine-learning workflows and models as web services.</span></span> <span data-ttu-id="6ca72-105">Dessa webbtjänster kan sedan användas för att anropa machine learning-modeller från program via Internet för att göra förutsägelser i realtid eller i batchläge.</span><span class="sxs-lookup"><span data-stu-id="6ca72-105">These web services can then be used to call the machine-learning models from applications over the Internet to do predictions in real time or in batch mode.</span></span> <span data-ttu-id="6ca72-106">Eftersom webbtjänsterna RESTful kan anropa du dem från olika programmeringsspråk språk och plattformar, till exempel .NET och Java, och program som Excel.</span><span class="sxs-lookup"><span data-stu-id="6ca72-106">Because the web services are RESTful, you can call them from various programming languages and platforms, such as .NET and Java, and from applications, such as Excel.</span></span>

<span data-ttu-id="6ca72-107">Nästa avsnitt innehåller länkar till genomgång, kod och dokumentation för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="6ca72-107">The next sections provide links to walkthroughs, code, and documentation to help get you started.</span></span>

## <a name="deploy-a-web-service"></a><span data-ttu-id="6ca72-108">Distribuera en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6ca72-108">Deploy a web service</span></span>
### <a name="with-azure-machine-learning-studio"></a><span data-ttu-id="6ca72-109">Med Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="6ca72-109">With Azure Machine Learning Studio</span></span>
<span data-ttu-id="6ca72-110">Machine Learning Studio och Microsoft Azure Machine Learning-webbtjänster portalen kan du distribuera och hantera en webbtjänst utan att skriva kod.</span><span class="sxs-lookup"><span data-stu-id="6ca72-110">Machine Learning Studio and the Microsoft Azure Machine Learning Web Services portal help you deploy and manage a web service without writing code.</span></span>

<span data-ttu-id="6ca72-111">Följande länkar ger allmän Information om hur du distribuerar en ny webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="6ca72-111">The following links provide general Information about how to deploy a new web service:</span></span>

* <span data-ttu-id="6ca72-112">Översiktligt om hur du distribuerar en ny webbtjänst som baseras på Azure Resource Manager finns [distribuera en ny webbtjänst](machine-learning-webservice-deploy-a-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6ca72-112">For an overview about how to deploy a new web service that's based on Azure Resource Manager, see [Deploy a new web service](machine-learning-webservice-deploy-a-web-service.md).</span></span>
* <span data-ttu-id="6ca72-113">En genomgång om hur du distribuerar en webbtjänst finns [distribuera en Azure Machine Learning-webbtjänst](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6ca72-113">For a walkthrough about how to deploy a web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="6ca72-114">En fullständig genomgång om hur du skapar och distribuerar en webbtjänst finns [genomgången steg 1: skapa en arbetsyta i Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="6ca72-114">For a full walkthrough about how to create and deploy a web service, see [Walkthrough Step 1: Create a Machine Learning workspace](machine-learning-walkthrough-1-create-ml-workspace.md).</span></span>
* <span data-ttu-id="6ca72-115">Vissa exempel som distribuerar en webbtjänst finns:</span><span class="sxs-lookup"><span data-stu-id="6ca72-115">For specific examples that deploy a web service, see:</span></span>

  * [<span data-ttu-id="6ca72-116">Genomgång steg 5: Distribuera Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6ca72-116">Walkthrough Step 5: Deploy the Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)
  * [<span data-ttu-id="6ca72-117">Distribuera en webbtjänst till flera regioner</span><span class="sxs-lookup"><span data-stu-id="6ca72-117">How to deploy a web service to multiple regions</span></span>](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a><span data-ttu-id="6ca72-118">Med resursprovidern för web services API: er (Azure Resource Manager API: er)</span><span class="sxs-lookup"><span data-stu-id="6ca72-118">With web services resource provider APIs (Azure Resource Manager APIs)</span></span>
<span data-ttu-id="6ca72-119">Azure Machine Learning resource provider för webbtjänster kan distribution och hantering av webbtjänster med hjälp av REST API-anrop.</span><span class="sxs-lookup"><span data-stu-id="6ca72-119">The Azure Machine Learning resource provider for web services enables deployment and management of web services by using REST API calls.</span></span> <span data-ttu-id="6ca72-120">Mer information finns i [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) referens.</span><span class="sxs-lookup"><span data-stu-id="6ca72-120">For additional details, see the [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) reference.</span></span>

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a><span data-ttu-id="6ca72-121">Med PowerShell-cmdlets</span><span class="sxs-lookup"><span data-stu-id="6ca72-121">With PowerShell cmdlets</span></span>
<span data-ttu-id="6ca72-122">Azure Machine Learning resource provider för webbtjänster kan distribution och hantering av webbtjänster med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="6ca72-122">Azure Machine Learning resource provider for web services enables deployment and management of web services by using PowerShell cmdlets.</span></span>

<span data-ttu-id="6ca72-123">Om du vill använda cmdlets måste du först logga in på ditt Azure-konto från PowerShell-miljö med hjälp av den [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6ca72-123">To use the cmdlets, you must first sign in to your Azure account from within the PowerShell environment by using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="6ca72-124">Om du inte känner till hur du anropar PowerShell-kommandon som är baserade på hanteraren för filserverresurser, se [med hjälp av Azure PowerShell med Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span><span class="sxs-lookup"><span data-stu-id="6ca72-124">If you are unfamiliar with how to call PowerShell commands that are based on Resource Manager, see [Using Azure PowerShell with Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).</span></span>

<span data-ttu-id="6ca72-125">Om du vill exportera dina prediktivt experiment, Använd [den här exempelkoden](https://github.com/ritwik20/AzureML-WebServices).</span><span class="sxs-lookup"><span data-stu-id="6ca72-125">To export your predictive experiment, use [this sample code](https://github.com/ritwik20/AzureML-WebServices).</span></span> <span data-ttu-id="6ca72-126">När du skapar .exe-fil från koden, skriver du:</span><span class="sxs-lookup"><span data-stu-id="6ca72-126">After you create the .exe file from the code, you can type:</span></span>

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

<span data-ttu-id="6ca72-127">Kör programmet skapar en mall för JSON web.</span><span class="sxs-lookup"><span data-stu-id="6ca72-127">Running the application creates a web service JSON template.</span></span> <span data-ttu-id="6ca72-128">Om du vill använda mallen för att distribuera en webbtjänst, måste du lägga till följande information:</span><span class="sxs-lookup"><span data-stu-id="6ca72-128">To use the template to deploy a web service, you must add the following information:</span></span>

* <span data-ttu-id="6ca72-129">Lagringskontonamn och nyckel</span><span class="sxs-lookup"><span data-stu-id="6ca72-129">Storage account name and key</span></span>

    <span data-ttu-id="6ca72-130">Du kan hämta lagringskontonamn och nyckel från antingen den [Azure-portalen](https://portal.azure.com/) eller [klassiska Azure-portalen](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="6ca72-130">You can get the storage account name and key from either the [Azure portal](https://portal.azure.com/) or the [Azure classic portal](http://manage.windowsazure.com/).</span></span>
* <span data-ttu-id="6ca72-131">Åtagande plan-ID</span><span class="sxs-lookup"><span data-stu-id="6ca72-131">Commitment plan ID</span></span>

    <span data-ttu-id="6ca72-132">Du kan hämta plan-ID från den [Azure Machine Learning-webbtjänster](https://services.azureml.net) portal genom att logga in och klicka på ett namn för energischema.</span><span class="sxs-lookup"><span data-stu-id="6ca72-132">You can get the plan ID from the [Azure Machine Learning Web Services](https://services.azureml.net) portal by signing in and clicking a plan name.</span></span>

<span data-ttu-id="6ca72-133">Lägg till dem i JSON-mall som underordnade till den *egenskaper* nod på samma nivå som den *MachineLearningWorkspace* nod.</span><span class="sxs-lookup"><span data-stu-id="6ca72-133">Add them to the JSON template as children of the *Properties* node at the same level as the *MachineLearningWorkspace* node.</span></span>

<span data-ttu-id="6ca72-134">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="6ca72-134">Here's an example:</span></span>

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

<span data-ttu-id="6ca72-135">Se följande artiklar och exempelkod för ytterligare information:</span><span class="sxs-lookup"><span data-stu-id="6ca72-135">See the following articles and sample code for additional details:</span></span>

* <span data-ttu-id="6ca72-136">[Azure Machine Learning-Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference på MSDN</span><span class="sxs-lookup"><span data-stu-id="6ca72-136">[Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN</span></span>
* <span data-ttu-id="6ca72-137">Exempel [genomgången](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) på GitHub</span><span class="sxs-lookup"><span data-stu-id="6ca72-137">Sample [walkthrough](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) on GitHub</span></span>

## <a name="consume-the-web-services"></a><span data-ttu-id="6ca72-138">Konsumera webbtjänster</span><span class="sxs-lookup"><span data-stu-id="6ca72-138">Consume the web services</span></span>
### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a><span data-ttu-id="6ca72-139">Från Azure Machine Learning-webbtjänster Användargränssnittet (test)</span><span class="sxs-lookup"><span data-stu-id="6ca72-139">From the Azure Machine Learning Web Services UI (Testing)</span></span>
<span data-ttu-id="6ca72-140">Du kan testa webbtjänsten från portalen Azure Machine Learning-webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="6ca72-140">You can test your web service from the Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="6ca72-141">Detta inkluderar testar tjänsten begäran och svar (RR) och Batch Execution service (BES)-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="6ca72-141">This includes testing the Request-Response service (RRS) and Batch Execution service (BES) interfaces.</span></span>

* [<span data-ttu-id="6ca72-142">Distribuera en ny webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6ca72-142">Deploy a new web service</span></span>](machine-learning-webservice-deploy-a-web-service.md)
* [<span data-ttu-id="6ca72-143">Distribuera en Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6ca72-143">Deploy an Azure Machine Learning web service</span></span>](machine-learning-publish-a-machine-learning-web-service.md)
* [<span data-ttu-id="6ca72-144">Genomgång steg 5: Distribuera Azure Machine Learning-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="6ca72-144">Walkthrough Step 5: Deploy the Azure Machine Learning web service</span></span>](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a><span data-ttu-id="6ca72-145">Från Excel</span><span class="sxs-lookup"><span data-stu-id="6ca72-145">From Excel</span></span>
<span data-ttu-id="6ca72-146">Du kan hämta en Excel-mall som förbrukar webbtjänsten:</span><span class="sxs-lookup"><span data-stu-id="6ca72-146">You can download an Excel template that consumes the web service:</span></span>

* [<span data-ttu-id="6ca72-147">Använda en Azure Machine Learning-webbtjänst från Excel</span><span class="sxs-lookup"><span data-stu-id="6ca72-147">Consuming an Azure Machine Learning web service from Excel</span></span>](machine-learning-consuming-from-excel.md)
* [<span data-ttu-id="6ca72-148">Excel-tillägg för Azure Machine Learning-webbtjänster</span><span class="sxs-lookup"><span data-stu-id="6ca72-148">Excel add-in for Azure Machine Learning Web Services</span></span>](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a><span data-ttu-id="6ca72-149">En REST-baserad klient</span><span class="sxs-lookup"><span data-stu-id="6ca72-149">From a REST-based client</span></span>
<span data-ttu-id="6ca72-150">Azure Machine Learning-webbtjänster är RESTful-API: er.</span><span class="sxs-lookup"><span data-stu-id="6ca72-150">Azure Machine Learning Web Services are RESTful APIs.</span></span> <span data-ttu-id="6ca72-151">Du kan använda dessa API: er från olika plattformar, till exempel .NET, Python, R, Java och så vidare. Den **förbruka** sidan för webbtjänsten på den [Microsoft Azure Machine Learning-webbtjänster portal](https://services.azureml.net) har exempelkod som kan hjälpa dig att komma igång.</span><span class="sxs-lookup"><span data-stu-id="6ca72-151">You can consume these APIs from various platforms, such as .NET, Python, R, Java, etc. The **Consume** page for your web service on the [Microsoft Azure Machine Learning Web Services portal](https://services.azureml.net) has sample code that can help you get started.</span></span> <span data-ttu-id="6ca72-152">Mer information finns i [Använda Azure Machine Learning-webbtjänster](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="6ca72-152">For more information, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>
