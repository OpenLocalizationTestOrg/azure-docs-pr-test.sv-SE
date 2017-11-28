---
title: aaaConsume Azure hanterade program i marketplace | Microsoft Docs
description: "Describeshow toocreate ett Azure hanterade program som är tillgängliga via hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 9ae6e11a3f63eb58a9f3199364b5606a7afe5618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-azure-managed-applications-in-hello-marketplace"></a><span data-ttu-id="415c7-103">Använda Azure hanterade program i hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="415c7-103">Consume Azure managed applications in hello Marketplace</span></span>

<span data-ttu-id="415c7-104">Enligt beskrivningen i hello [hanteras Programöversikt](managed-application-overview.md) artikel, det finns två scenarier i hello slutet tooend upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="415c7-104">As discussed in hello [Managed Application overview](managed-application-overview.md) article, there are two scenarios in hello end tooend experience.</span></span> <span data-ttu-id="415c7-105">En är hello utgivare eller leverantören som vill toocreate ett hanterat program för användning av kunder.</span><span class="sxs-lookup"><span data-stu-id="415c7-105">One is hello publisher or vendor who wants toocreate a managed application for use by customers.</span></span> <span data-ttu-id="415c7-106">hello är andra hello slutet kund eller hello konsumenter av hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="415c7-106">hello second is hello end customer or hello consumer of hello managed application.</span></span> <span data-ttu-id="415c7-107">Den här artikeln beskriver hello andra scenariot.</span><span class="sxs-lookup"><span data-stu-id="415c7-107">This article covers hello second scenario.</span></span> <span data-ttu-id="415c7-108">Beskriver hur du distribuerar ett hanterat program från hello Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="415c7-108">It describes how you can deploy a managed application from hello Microsoft Azure Marketplace.</span></span>

## <a name="create-from-hello-marketplace"></a><span data-ttu-id="415c7-109">Skapa från hello Marketplace</span><span class="sxs-lookup"><span data-stu-id="415c7-109">Create from hello Marketplace</span></span>

<span data-ttu-id="415c7-110">Distribuera ett hanterat program från hello Marketplace är liknande toodeploying någon typ av resurser från hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="415c7-110">Deploying a managed application from hello Marketplace is similar toodeploying any type of resources from hello Marketplace.</span></span> 

<span data-ttu-id="415c7-111">I hello portal, väljer **+ ny** och Sök efter hello typ av lösningen toodeploy.</span><span class="sxs-lookup"><span data-stu-id="415c7-111">In hello portal, select **+ New** and search for hello type of solution toodeploy.</span></span> <span data-ttu-id="415c7-112">Hello tillgängliga alternativ, Välj hello som du behöver.</span><span class="sxs-lookup"><span data-stu-id="415c7-112">From hello available options, select hello one you need.</span></span>

![söklösningar](./media/managed-application-consume-marketplace/search-apps.png)

<span data-ttu-id="415c7-114">Granska hello sammanfattning av programmet hello och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="415c7-114">Review hello summary of hello application, and select **Create**.</span></span>

![Skapa hanterade program](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

<span data-ttu-id="415c7-116">Precis som alla andra lösningar visas för fält tooprovide värden för.</span><span class="sxs-lookup"><span data-stu-id="415c7-116">Like any other solution, you are presented with fields tooprovide values for.</span></span> <span data-ttu-id="415c7-117">De här fälten varierar beroende på hello typ av hanterade program som du skapar.</span><span class="sxs-lookup"><span data-stu-id="415c7-117">These fields vary by hello type of managed application you create.</span></span> 

## <a name="view-support-information"></a><span data-ttu-id="415c7-118">Visa information om support</span><span class="sxs-lookup"><span data-stu-id="415c7-118">View support information</span></span>

<span data-ttu-id="415c7-119">Visa hello supportinformation för hello programmet när det hanterade programmet har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="415c7-119">After your managed application has deployed, view hello support information for hello application.</span></span> <span data-ttu-id="415c7-120">I bladet hanterade programmet hello visas hello supportinformation.</span><span class="sxs-lookup"><span data-stu-id="415c7-120">In hello managed application blade, hello support information is listed.</span></span>

![Support](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a><span data-ttu-id="415c7-122">Visa publisher behörigheter</span><span class="sxs-lookup"><span data-stu-id="415c7-122">View publisher permissions</span></span>

<span data-ttu-id="415c7-123">hello-leverantör som hanterar ditt program har beviljats åtkomst tooyour resurser.</span><span class="sxs-lookup"><span data-stu-id="415c7-123">hello vendor that manages your application is granted access tooyour resources.</span></span> <span data-ttu-id="415c7-124">toosee dessa behörigheter och välj **tillstånd**.</span><span class="sxs-lookup"><span data-stu-id="415c7-124">toosee those permissions, select **Authorizations**.</span></span>

![Tillstånd](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a><span data-ttu-id="415c7-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="415c7-126">Next steps</span></span>

* <span data-ttu-id="415c7-127">Information om hur du publicerar ett hanterat program i hello Marketplace finns [Azure hanterade program i Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="415c7-127">For information about publishing a managed application in hello Marketplace, see [Azure Managed Applications in Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="415c7-128">toopublish hanterade program som är endast tillgänglig toousers i din organisation, se [skapa och publicera katalog hanteras tjänstprogrammet](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="415c7-128">toopublish managed applications that are only available toousers in your organization, see [Create and publish Service Catalog Managed Application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="415c7-129">tooconsume hanterade program som är endast tillgänglig toousers i din organisation, se [använda en katalog hanteras tjänstprogrammet](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="415c7-129">tooconsume managed applications that are only available toousers in your organization, see [Consume a Service Catalog Managed Application](managed-application-consumption.md).</span></span>
