---
title: "aaaHow toodeploy en webbtjänst toomultiple regioner | Microsoft Docs"
description: "Steg toodeploy (kopiera) en ny webbtjänst tooother regioner."
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
ms.openlocfilehash: 21fcdb96f118c60ed98b60b1b2df833766c7c8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-web-service-toomultiple-regions"></a><span data-ttu-id="d96e4-103">Hur toodeploy en webbtjänst toomultiple regioner</span><span class="sxs-lookup"><span data-stu-id="d96e4-103">How toodeploy a Web Service toomultiple regions</span></span>
<span data-ttu-id="d96e4-104">hello nya Azure-webbtjänster kan du tooeasily distribuera en web service toomultiple regioner utan att behöva flera prenumerationer eller arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="d96e4-104">hello New Azure Web Services allow you tooeasily deploy a web service toomultiple regions without needing multiple subscriptions or workspaces.</span></span> 

<span data-ttu-id="d96e4-105">Priser är regionspecifika, därför måste du definiera ett faktureringsavtal för varje region där du ska distribuera hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d96e4-105">Pricing is region specific, therefore you must define a billing plan for each region in which you will deploy hello web service.</span></span>

## <a name="toocreate-a-plan-in-another-region"></a><span data-ttu-id="d96e4-106">toocreate en plan i en annan region</span><span class="sxs-lookup"><span data-stu-id="d96e4-106">toocreate a plan in another region</span></span>
1. <span data-ttu-id="d96e4-107">Logga in på [Microsoft Azure Machine Learning-webbtjänster](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="d96e4-107">Sign into [Microsoft Azure Machine Learning Web Services](https://services.azureml.net/).</span></span>
2. <span data-ttu-id="d96e4-108">Klicka på hello **planer** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="d96e4-108">Click hello **Plans** menu option.</span></span>
3. <span data-ttu-id="d96e4-109">Klicka på hello planer över visa sidan, **ny**.</span><span class="sxs-lookup"><span data-stu-id="d96e4-109">On hello Plans over view page, click **New**.</span></span>
4. <span data-ttu-id="d96e4-110">Från hello **prenumeration** listrutan, Välj hello prenumeration i vilka hello ny plan kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="d96e4-110">From hello **Subscription** dropdown, select hello subscription in which hello new plan will reside.</span></span>
5. <span data-ttu-id="d96e4-111">Från hello **Region** listrutan, Välj en region för hello ny plan.</span><span class="sxs-lookup"><span data-stu-id="d96e4-111">From hello **Region** dropdown, select a region for hello new plan.</span></span> <span data-ttu-id="d96e4-112">hello planera alternativ för hello valda regionen visas i hello **planera alternativ** på hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="d96e4-112">hello Plan Options for hello selected region will display in hello **Plan Options** section of hello page.</span></span>
6. <span data-ttu-id="d96e4-113">Från hello **resursgruppen** listrutan, Välj en resursgrupp för hello-plan.</span><span class="sxs-lookup"><span data-stu-id="d96e4-113">From hello **Resource Group** dropdown, select a resource group for hello plan.</span></span> <span data-ttu-id="d96e4-114">För mer information om resursgrupper finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d96e4-114">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
7. <span data-ttu-id="d96e4-115">I **schemanamn** hello-typnamn för hello plan.</span><span class="sxs-lookup"><span data-stu-id="d96e4-115">In **Plan Name** type hello name of hello plan.</span></span>
8. <span data-ttu-id="d96e4-116">Under **alternativ**, klicka på hello fakturering nivå för hello ny plan.</span><span class="sxs-lookup"><span data-stu-id="d96e4-116">Under **Plan Options**, click hello billing level for hello new plan.</span></span>
9. <span data-ttu-id="d96e4-117">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d96e4-117">Click **Create**.</span></span>

## <a name="deploying-hello-web-service-tooanother-region"></a><span data-ttu-id="d96e4-118">Distribuera hello web service tooanother region</span><span class="sxs-lookup"><span data-stu-id="d96e4-118">Deploying hello web service tooanother region</span></span>
1. <span data-ttu-id="d96e4-119">Klicka på hello **Web Services** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="d96e4-119">Click hello **Web Services** menu option.</span></span>
2. <span data-ttu-id="d96e4-120">Välj hello webbtjänst som du distribuerar tooa ny region.</span><span class="sxs-lookup"><span data-stu-id="d96e4-120">Select hello Web Service you are deploying tooa new region.</span></span>
3. <span data-ttu-id="d96e4-121">Klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="d96e4-121">Click **Copy**.</span></span>
4. <span data-ttu-id="d96e4-122">I **Webbtjänstnamn**, ange ett nytt namn för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d96e4-122">In **Web Service Name**, type a new name for hello web service.</span></span>
5. <span data-ttu-id="d96e4-123">I **Web tjänstbeskrivningen**, ange en beskrivning för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d96e4-123">In **Web service description**, type a description for hello web service.</span></span>
6. <span data-ttu-id="d96e4-124">Från hello **prenumeration** listrutan, Välj hello prenumeration i vilka hello nya webbtjänsten kommer att finnas.</span><span class="sxs-lookup"><span data-stu-id="d96e4-124">From hello **Subscription** dropdown, select hello subscription in which hello new web service will reside.</span></span>
7. <span data-ttu-id="d96e4-125">Från hello **resursgruppen** listrutan, Välj en resursgrupp för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d96e4-125">From hello **Resource Group** dropdown, select a resource group for hello web service.</span></span> <span data-ttu-id="d96e4-126">För mer information om resursgrupper finns [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d96e4-126">From more information on resource groups, see [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
8. <span data-ttu-id="d96e4-127">Från hello **Region** listrutan, Välj hello region i vilken toodeploy hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d96e4-127">From hello **Region** dropdown, select hello region in which toodeploy hello web service.</span></span>
9. <span data-ttu-id="d96e4-128">Från hello **lagringskonto** listrutan, Välj ett lagringskonto i vilka toostore hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="d96e4-128">From hello **Storage account** dropdown, select a storage account in which toostore hello web service.</span></span>
10. <span data-ttu-id="d96e4-129">Från hello **prisplan** listrutan en plan i hello region som du valde i steg 8.</span><span class="sxs-lookup"><span data-stu-id="d96e4-129">From hello **Price Plan** dropdown, select a plan in hello region you selected in step 8.</span></span>
11. <span data-ttu-id="d96e4-130">Klicka på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="d96e4-130">Click **Copy**.</span></span>

