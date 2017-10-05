---
title: "Vad är nytt i Azure Machine Learning | Microsoft Docs"
description: "Nya funktioner som är tillgängliga i Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: ddc716ed-2615-4806-bf27-6c9a5662a7f2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 551b977b90612ddbfa1514a9c2358ebf8179c385
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-azure-machine-learning"></a><span data-ttu-id="a6a52-103">Vad är nytt i Azure Machine Learning?</span><span class="sxs-lookup"><span data-stu-id="a6a52-103">What's New in Azure Machine Learning</span></span>

### <a name="the-march-2017-release-of-microsoft-azure-machine-learning-updates-provides-the-following-feature"></a><span data-ttu-id="a6a52-104">Mars 2017-versionen av Microsoft Azure Machine Learning uppdateringar innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a6a52-104">The March 2017 release of Microsoft Azure Machine Learning updates provides the following feature:</span></span>



* <span data-ttu-id="a6a52-105">Dedikerade kapacitet för Azure Machine Learning BES-jobb</span><span class="sxs-lookup"><span data-stu-id="a6a52-105">Dedicated Capacity for Azure Machine Learning BES Jobs</span></span>

    <span data-ttu-id="a6a52-106">Datorn Learning Batch-Pool bearbetning använder den [Azure Batch](../batch/batch-technical-overview.md) ska hanteras av kunden skalan för Azure Machine Learning Batch Execution Service.</span><span class="sxs-lookup"><span data-stu-id="a6a52-106">Machine Learning Batch Pool processing uses the [Azure Batch](../batch/batch-technical-overview.md) service to provide customer-managed scale for the Azure Machine Learning Batch Execution Service.</span></span> <span data-ttu-id="a6a52-107">Poolen batchbearbetning kan du skapa Azure Batch-pooler som du kan skicka batchjobb och ha dem köra förutsägbart.</span><span class="sxs-lookup"><span data-stu-id="a6a52-107">Batch Pool processing allows you to create Azure Batch pools on which you can submit batch jobs and have them execute in a predictable manner.</span></span>

    <span data-ttu-id="a6a52-108">Mer information finns i [Azure Batch-tjänsten för Machine Learning jobb](machine-learning-dedicated-capacity-for-bes-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="a6a52-108">For more information, see [Azure Batch service for Machine Learning jobs](machine-learning-dedicated-capacity-for-bes-jobs.md).</span></span>


### <a name="the-august-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a><span data-ttu-id="a6a52-109">Augusti 2016-versionen av Microsoft Azure Machine Learning uppdateringar innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a6a52-109">The August 2016 release of Microsoft Azure Machine Learning updates provide the following features:</span></span>
* <span data-ttu-id="a6a52-110">Klassiska webbtjänster kan hanteras i den nya [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/) portal som ger dig en plats för att hantera alla aspekter av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6a52-110">Classic Web services can now be managed in the new [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/) portal that provides one place to manage all aspects of your Web service.</span></span>    
  * <span data-ttu-id="a6a52-111">Som innehåller webbtjänsten [användningsstatistik](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="a6a52-111">Which provides web service [usage statistics](machine-learning-manage-new-webservice.md).</span></span>
  * <span data-ttu-id="a6a52-112">Förenklar testning av Azure Machine Learning fjärr-begäran-anrop med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="a6a52-112">Simplifies testing of Azure Machine Learning Remote-Request calls using sample data.</span></span>
  * <span data-ttu-id="a6a52-113">Ger en ny Batch Execution Service testsida exempel data och jobbet skicka historik.</span><span class="sxs-lookup"><span data-stu-id="a6a52-113">Provides a new Batch Execution Service test page with sample data and job submission history.</span></span>
  * <span data-ttu-id="a6a52-114">Tillhandahåller enklare hantering av slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a6a52-114">Provides easier endpoint management.</span></span>

### <a name="the-july-2016-release-of-microsoft-azure-machine-learning-updates-provide-the-following-features"></a><span data-ttu-id="a6a52-115">Juli 2016-versionen av Microsoft Azure Machine Learning uppdateringar innehåller följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="a6a52-115">The July 2016 release of Microsoft Azure Machine Learning updates provide the following features:</span></span>
* <span data-ttu-id="a6a52-116">Webbtjänster som nu hanteras som Azure-resurser som hanteras via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) gränssnitt, vilket ger följande förbättringar:</span><span class="sxs-lookup"><span data-stu-id="a6a52-116">Web services are now managed as Azure resources managed through [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) interfaces, allowing for the following enhancements:</span></span>
  * <span data-ttu-id="a6a52-117">Det finns nya [REST API: er](https://msdn.microsoft.com/library/azure/Dn950030.aspx) att distribuera och hantera Resource Manager baserat webbtjänsterna.</span><span class="sxs-lookup"><span data-stu-id="a6a52-117">There are new [REST APIs](https://msdn.microsoft.com/library/azure/Dn950030.aspx) to deploy and manage your Resource Manager based Web services.</span></span>
  * <span data-ttu-id="a6a52-118">Det finns en ny [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/) portal som ger dig en plats för att hantera alla aspekter av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a6a52-118">There is a new [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/) portal that provides one place to manage all aspects of your Web service.</span></span>
* <span data-ttu-id="a6a52-119">Innehåller en ny prenumeration-baserade, flera regioner web service distributionsmodell med Resource Manager baserade API: er som utnyttjar Resource Manager Resource Provider för webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="a6a52-119">Incorporates a new subscription-based, multi-region web service deployment model using Resource Manager based APIs leveraging the Resource Manager Resource Provider for Web Services.</span></span>
* <span data-ttu-id="a6a52-120">Inför nya [prissättningar](https://azure.microsoft.com/pricing/details/machine-learning/) och planera hanteringsmöjligheter med nya Resource Manager RP för fakturering.</span><span class="sxs-lookup"><span data-stu-id="a6a52-120">Introduces new [pricing plans](https://azure.microsoft.com/pricing/details/machine-learning/) and plan management capabilities using the new Resource Manager RP for Billing.</span></span>
  * <span data-ttu-id="a6a52-121">Du kan nu [distribuera webbtjänsten till flera regioner](machine-learning-how-to-deploy-to-multiple-regions.md) utan att behöva skapa en prenumeration i varje region.</span><span class="sxs-lookup"><span data-stu-id="a6a52-121">You can now [deploy your web service to multiple regions](machine-learning-how-to-deploy-to-multiple-regions.md) without needing to create a subscription in each region.</span></span>
* <span data-ttu-id="a6a52-122">Innehåller webbtjänsten [användningsstatistik](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="a6a52-122">Provides web service [usage statistics](machine-learning-manage-new-webservice.md).</span></span>
* <span data-ttu-id="a6a52-123">Förenklar testning av Azure Machine Learning fjärr-begäran-anrop med exempeldata.</span><span class="sxs-lookup"><span data-stu-id="a6a52-123">Simplifies testing of Azure Machine Learning Remote-Request calls using sample data.</span></span>
* <span data-ttu-id="a6a52-124">Ger en ny Batch Execution Service testsida exempel data och jobbet skicka historik.</span><span class="sxs-lookup"><span data-stu-id="a6a52-124">Provides a new Batch Execution Service test page with sample data and job submission history.</span></span>

<span data-ttu-id="a6a52-125">Dessutom har uppdaterats så att du kan distribuera till den nya Web service modellen Machine Learning Studio eller fortsätta att distribuera till den klassiska modellen för Web service.</span><span class="sxs-lookup"><span data-stu-id="a6a52-125">In addition, the Machine Learning Studio has been updated to allow you to deploy to the new Web service model or continue to deploy to the classic Web service model.</span></span> 

