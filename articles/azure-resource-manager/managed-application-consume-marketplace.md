---
title: "Använda Azure hanterade program i marketplace | Microsoft Docs"
description: "Describeshow skapa en Azure-hanterad App som är tillgängligt via Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: baf456740bddd562391ed64d707f990c8921d710
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="consume-azure-managed-applications-in-the-marketplace"></a><span data-ttu-id="0968e-103">Använda Azure hanterade program i Marketplace</span><span class="sxs-lookup"><span data-stu-id="0968e-103">Consume Azure managed applications in the Marketplace</span></span>

<span data-ttu-id="0968e-104">Enligt beskrivningen i den [hanteras Programöversikt](managed-application-overview.md) artikel, det finns två scenarier i slutpunkt till slutpunkt-upplevelsen.</span><span class="sxs-lookup"><span data-stu-id="0968e-104">As discussed in the [Managed Application overview](managed-application-overview.md) article, there are two scenarios in the end to end experience.</span></span> <span data-ttu-id="0968e-105">En är utgivare eller leverantören som vill skapa ett hanterat program för användning av kunder.</span><span class="sxs-lookup"><span data-stu-id="0968e-105">One is the publisher or vendor who wants to create a managed application for use by customers.</span></span> <span data-ttu-id="0968e-106">Andra är slutkunden eller förbrukare som det hanterade programmet.</span><span class="sxs-lookup"><span data-stu-id="0968e-106">The second is the end customer or the consumer of the managed application.</span></span> <span data-ttu-id="0968e-107">Den här artikeln beskrivs i det andra scenariot.</span><span class="sxs-lookup"><span data-stu-id="0968e-107">This article covers the second scenario.</span></span> <span data-ttu-id="0968e-108">Beskriver hur du distribuerar ett hanterat program från Microsoft Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0968e-108">It describes how you can deploy a managed application from the Microsoft Azure Marketplace.</span></span>

## <a name="create-from-the-marketplace"></a><span data-ttu-id="0968e-109">Skapa från Marketplace</span><span class="sxs-lookup"><span data-stu-id="0968e-109">Create from the Marketplace</span></span>

<span data-ttu-id="0968e-110">Distribuera ett hanterat program från Marketplace liknar distribuerar alla typer av resurser från Marketplace.</span><span class="sxs-lookup"><span data-stu-id="0968e-110">Deploying a managed application from the Marketplace is similar to deploying any type of resources from the Marketplace.</span></span> 

<span data-ttu-id="0968e-111">I portalen, Välj **+ ny** och Sök efter typ av lösningen för att distribuera.</span><span class="sxs-lookup"><span data-stu-id="0968e-111">In the portal, select **+ New** and search for the type of solution to deploy.</span></span> <span data-ttu-id="0968e-112">Välj det du behöver från de tillgängliga alternativen.</span><span class="sxs-lookup"><span data-stu-id="0968e-112">From the available options, select the one you need.</span></span>

![söklösningar](./media/managed-application-consume-marketplace/search-apps.png)

<span data-ttu-id="0968e-114">Granska sammanfattningen av programmet och välj **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0968e-114">Review the summary of the application, and select **Create**.</span></span>

![Skapa hanterade program](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

<span data-ttu-id="0968e-116">Precis som alla andra lösningar visas med fält för att ange värden för.</span><span class="sxs-lookup"><span data-stu-id="0968e-116">Like any other solution, you are presented with fields to provide values for.</span></span> <span data-ttu-id="0968e-117">De här fälten varierar beroende på vilken typ av hanterade program som du skapar.</span><span class="sxs-lookup"><span data-stu-id="0968e-117">These fields vary by the type of managed application you create.</span></span> 

## <a name="view-support-information"></a><span data-ttu-id="0968e-118">Visa information om support</span><span class="sxs-lookup"><span data-stu-id="0968e-118">View support information</span></span>

<span data-ttu-id="0968e-119">Visa supportinformation för programmet när det hanterade programmet har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="0968e-119">After your managed application has deployed, view the support information for the application.</span></span> <span data-ttu-id="0968e-120">Supportinformation visas i bladet hanterade program.</span><span class="sxs-lookup"><span data-stu-id="0968e-120">In the managed application blade, the support information is listed.</span></span>

![Support](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a><span data-ttu-id="0968e-122">Visa publisher behörigheter</span><span class="sxs-lookup"><span data-stu-id="0968e-122">View publisher permissions</span></span>

<span data-ttu-id="0968e-123">Leverantören som hanterar ditt program har beviljats åtkomst till resurser.</span><span class="sxs-lookup"><span data-stu-id="0968e-123">The vendor that manages your application is granted access to your resources.</span></span> <span data-ttu-id="0968e-124">Om du vill se de behörigheterna, Välj **tillstånd**.</span><span class="sxs-lookup"><span data-stu-id="0968e-124">To see those permissions, select **Authorizations**.</span></span>

![Tillstånd](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a><span data-ttu-id="0968e-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0968e-126">Next steps</span></span>

* <span data-ttu-id="0968e-127">Information om hur du publicerar ett hanterat program i Marketplace finns [Azure hanterade program i Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="0968e-127">For information about publishing a managed application in the Marketplace, see [Azure Managed Applications in Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="0968e-128">För att publicera hanterade program som bara är tillgängliga för användare i din organisation, se [skapa och publicera katalog hanteras tjänstprogrammet](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="0968e-128">To publish managed applications that are only available to users in your organization, see [Create and publish Service Catalog Managed Application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="0968e-129">Om du vill använda hanterade program som bara är tillgängliga för användare i din organisation, se [använda en katalog hanteras tjänstprogrammet](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="0968e-129">To consume managed applications that are only available to users in your organization, see [Consume a Service Catalog Managed Application](managed-application-consumption.md).</span></span>