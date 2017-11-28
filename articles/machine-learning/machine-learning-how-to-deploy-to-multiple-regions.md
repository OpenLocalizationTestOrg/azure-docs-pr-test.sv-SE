---
title: "Distribuera en webbtjänst till flera regioner | Microsoft Docs"
description: "Steg för att distribuera (kopiera) en ny webbtjänst till andra regioner."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 3895537bbca72e687838ff5013c291dfee3be707
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-deploy-a-web-service-to-multiple-regions"></a><span data-ttu-id="2399b-103">Så här distribuerar du en webbtjänst till flera regioner</span><span class="sxs-lookup"><span data-stu-id="2399b-103">How to deploy a Web Service to multiple regions</span></span>
<span data-ttu-id="2399b-104">Den nya Azure Web Services kan du enkelt distribuera en webbtjänst till flera regioner utan att behöva flera prenumerationer eller arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="2399b-104">The New Azure Web Services allow you to easily deploy a web service to multiple regions without needing multiple subscriptions or workspaces.</span></span> 

<span data-ttu-id="2399b-105">Priser är regionspecifika, därför måste du definiera ett faktureringsavtal för varje region där du ska distribuera webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2399b-105">Pricing is region specific, therefore you must define a billing plan for each region in which you will deploy the web service.</span></span>

## <a name="to-create-a-plan-in-another-region"></a><span data-ttu-id="2399b-106">Att skapa en plan i en annan region</span><span class="sxs-lookup"><span data-stu-id="2399b-106">To create a plan in another region</span></span>
1. <span data-ttu-id="2399b-107">Logga in på [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="2399b-107">Sign into [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).</span></span>
2. <span data-ttu-id="2399b-108">Klicka på den **planer** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="2399b-108">Click the **Plans** menu option.</span></span>
3. <span data-ttu-id="2399b-109">Klicka på planer över sidan Visa **ny**.</span><span class="sxs-lookup"><span data-stu-id="2399b-109">On the Plans over view page, click **New**.</span></span>
4. <span data-ttu-id="2399b-110">Från den **prenumeration** listrutan väljer du den prenumeration som den nya planen kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="2399b-110">From the **Subscription** dropdown, select the subscription in which the new plan will reside.</span></span>
5. <span data-ttu-id="2399b-111">Från den **Region** listrutan, Välj en region för den nya planen.</span><span class="sxs-lookup"><span data-stu-id="2399b-111">From the **Region** dropdown, select a region for the new plan.</span></span> <span data-ttu-id="2399b-112">Planera inställningar för den valda regionen visas i den **planera alternativ** avsnitt på sidan.</span><span class="sxs-lookup"><span data-stu-id="2399b-112">The Plan Options for the selected region will display in the **Plan Options** section of the page.</span></span>
6. <span data-ttu-id="2399b-113">Från den **resursgruppen** listrutan, Välj en resursgrupp för planen.</span><span class="sxs-lookup"><span data-stu-id="2399b-113">From the **Resource Group** dropdown, select a resource group for the plan.</span></span> <span data-ttu-id="2399b-114">För mer information om resursgrupper finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2399b-114">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
7. <span data-ttu-id="2399b-115">I **schemanamn** skriver du namnet på planen.</span><span class="sxs-lookup"><span data-stu-id="2399b-115">In **Plan Name** type the name of the plan.</span></span>
8. <span data-ttu-id="2399b-116">Under **alternativ**, klicka på nivån fakturering för den nya planen.</span><span class="sxs-lookup"><span data-stu-id="2399b-116">Under **Plan Options**, click the billing level for the new plan.</span></span>
9. <span data-ttu-id="2399b-117">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="2399b-117">Click **Create**.</span></span>

## <a name="deploying-the-web-service-to-another-region"></a><span data-ttu-id="2399b-118">Distribuera webbtjänsten till en annan region</span><span class="sxs-lookup"><span data-stu-id="2399b-118">Deploying the web service to another region</span></span>
1. <span data-ttu-id="2399b-119">Klicka på den **Web Services** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="2399b-119">Click the **Web Services** menu option.</span></span>
2. <span data-ttu-id="2399b-120">Välj den webbtjänst som du distribuerar till en ny region.</span><span class="sxs-lookup"><span data-stu-id="2399b-120">Select the Web Service you are deploying to a new region.</span></span>
3. <span data-ttu-id="2399b-121">Klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="2399b-121">Click **Copy**.</span></span>
4. <span data-ttu-id="2399b-122">I **Webbtjänstnamn**, ange ett nytt namn för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2399b-122">In **Web Service Name**, type a new name for the web service.</span></span>
5. <span data-ttu-id="2399b-123">I **Web tjänstbeskrivningen**, ange en beskrivning för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2399b-123">In **Web service description**, type a description for the web service.</span></span>
6. <span data-ttu-id="2399b-124">Från den **prenumeration** listrutan väljer du den prenumeration som den nya webbtjänsten kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="2399b-124">From the **Subscription** dropdown, select the subscription in which the new web service will reside.</span></span>
7. <span data-ttu-id="2399b-125">Från den **resursgruppen** listrutan, Välj en resursgrupp för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2399b-125">From the **Resource Group** dropdown, select a resource group for the web service.</span></span> <span data-ttu-id="2399b-126">För mer information om resursgrupper finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2399b-126">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="2399b-127">Från den **Region** listrutan väljer du den region där du vill distribuera webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2399b-127">From the **Region** dropdown, select the region in which to deploy the web service.</span></span>
9. <span data-ttu-id="2399b-128">Från den **lagringskonto** listrutan väljer du ett storage-konto för lagring av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="2399b-128">From the **Storage account** dropdown, select a storage account in which to store the web service.</span></span>
10. <span data-ttu-id="2399b-129">Från den **prisplan** listrutan, Välj en plan i den region som du valde i steg 8.</span><span class="sxs-lookup"><span data-stu-id="2399b-129">From the **Price Plan** dropdown, select a plan in the region you selected in step 8.</span></span>
11. <span data-ttu-id="2399b-130">Klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="2399b-130">Click **Copy**.</span></span>

